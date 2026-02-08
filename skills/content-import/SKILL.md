---
name: content-import
description: "Import existing website content from WordPress WXR/XML exports. Use when redesigning an existing site to preserve content. Triggers: 'import content', 'existing site', 'WordPress export', 'WXR', 'XML import', 'migrate content', 'redesign my site'."
---

# Content Import Skill

Import existing content from WordPress exports to use with new theme designs.

## When to Use

- User is redesigning an existing WordPress site
- User has a WordPress WXR/XML export file
- User wants to preview new theme with their real content (not lorem ipsum)

## Supported Formats

| Format | Extension | Source |
|--------|-----------|--------|
| WordPress WXR | `.xml` | Tools → Export in WordPress admin |
| WordPress.com Export | `.xml` | Settings → Export in WordPress.com |

## Workflow

### Step 1: Parse the Export File

```python
import xml.etree.ElementTree as ET
from html import unescape
import re

def parse_wxr(xml_path):
    """Parse WordPress WXR export file."""
    tree = ET.parse(xml_path)
    root = tree.getroot()

    # Namespace handling
    ns = {
        'wp': 'http://wordpress.org/export/1.2/',
        'content': 'http://purl.org/rss/1.0/modules/content/',
        'excerpt': 'http://wordpress.org/export/1.2/excerpt/',
    }

    content = {
        'site_title': '',
        'site_description': '',
        'posts': [],
        'pages': [],
        'categories': [],
        'tags': [],
        'menus': [],
    }

    # Site info
    channel = root.find('channel')
    content['site_title'] = channel.findtext('title', '')
    content['site_description'] = channel.findtext('description', '')

    # Parse items (posts, pages, attachments, nav_menu_items)
    for item in channel.findall('item'):
        post_type = item.findtext('wp:post_type', '', ns)
        status = item.findtext('wp:status', '', ns)

        if status != 'publish':
            continue

        entry = {
            'title': item.findtext('title', ''),
            'slug': item.findtext('wp:post_name', '', ns),
            'date': item.findtext('wp:post_date', '', ns),
            'content': unescape(item.findtext('content:encoded', '', ns) or ''),
            'excerpt': unescape(item.findtext('excerpt:encoded', '', ns) or ''),
        }

        if post_type == 'post':
            # Get categories and tags
            entry['categories'] = [c.text for c in item.findall('category[@domain="category"]')]
            entry['tags'] = [t.text for t in item.findall('category[@domain="post_tag"]')]
            content['posts'].append(entry)
        elif post_type == 'page':
            content['pages'].append(entry)
        elif post_type == 'nav_menu_item':
            content['menus'].append({
                'title': entry['title'],
                'url': item.findtext('wp:postmeta[wp:meta_key="menu_item_url"]/wp:meta_value', '', ns),
            })

    # Parse categories
    for cat in channel.findall('wp:category', ns):
        content['categories'].append({
            'name': cat.findtext('wp:cat_name', '', ns),
            'slug': cat.findtext('wp:category_nicename', '', ns),
        })

    # Parse tags
    for tag in channel.findall('wp:tag', ns):
        content['tags'].append({
            'name': tag.findtext('wp:tag_name', '', ns),
            'slug': tag.findtext('wp:tag_slug', '', ns),
        })

    return content
```

### Step 2: Present Content Summary

After parsing, show the user what was found:

```markdown
## Imported Content Summary

**Site:** [Site Title]
**Tagline:** [Site Description]

| Content Type | Count |
|--------------|-------|
| Posts | X |
| Pages | X |
| Categories | X |
| Tags | X |
| Menu Items | X |

### Pages
- Home
- About
- Contact
- Services

### Recent Posts
1. [Post Title] (Jan 15, 2024)
2. [Post Title] (Jan 10, 2024)
3. [Post Title] (Jan 5, 2024)

### Menu Structure
- Home
- About
- Services
  - Service A
  - Service B
- Contact
```

### Step 3: Use Content in Playground Blueprint

Add content to the blueprint so the theme preview shows real content:

```python
def create_content_steps(content):
    """Generate blueprint steps to import content."""
    steps = []

    # Create pages
    for page in content['pages']:
        steps.append({
            "step": "runPHP",
            "code": f'''<?php
require_once 'wp-load.php';
wp_insert_post([
    'post_title' => {repr(page['title'])},
    'post_name' => {repr(page['slug'])},
    'post_content' => {repr(page['content'])},
    'post_status' => 'publish',
    'post_type' => 'page',
]);
?>'''
        })

    # Create posts
    for post in content['posts'][:10]:  # Limit to 10 for preview
        steps.append({
            "step": "runPHP",
            "code": f'''<?php
require_once 'wp-load.php';
wp_insert_post([
    'post_title' => {repr(post['title'])},
    'post_name' => {repr(post['slug'])},
    'post_content' => {repr(post['content'])},
    'post_status' => 'publish',
    'post_type' => 'post',
]);
?>'''
        })

    # Set site title
    steps.append({
        "step": "runPHP",
        "code": f'''<?php
require_once 'wp-load.php';
update_option('blogname', {repr(content['site_title'])});
update_option('blogdescription', {repr(content['site_description'])});
?>'''
    })

    return steps
```

### Step 4: Integrate with Theme Blueprint

```python
# Full blueprint with theme + content
blueprint = {
    "$schema": "https://playground.wordpress.net/blueprint-schema.json",
    "landingPage": "/",
    "steps": [
        # Theme installation steps...
        {"step": "writeFile", "path": "...", "data": {"resource": "url", "url": f"data:application/zip;base64,{theme_data}"}},
        {"step": "unzip", ...},
        {"step": "activateTheme", ...},

        # Content import steps
        *create_content_steps(imported_content),

        # Set homepage
        {"step": "runPHP", "code": '''<?php
require_once 'wp-load.php';
$home = get_page_by_path('home');
if ($home) {
    update_option('show_on_front', 'page');
    update_option('page_on_front', $home->ID);
}
?>'''},
    ]
}
```

## Important Notes

1. **Content goes in database, not templates** — The theme should use dynamic blocks (`post-content`, `query`) to display this content
2. **Preview only** — Playground content is temporary; real import happens on the live site
3. **Limit content for preview** — Import ~10 posts max to keep blueprint size reasonable
4. **Preserve structure** — Note the menu structure and page hierarchy for navigation patterns

## Integration with Other Skills

After importing content:
1. Use `design-mockups` skill to create designs informed by actual content structure
2. Use `wp-block-themes` skill to build theme with dynamic templates
3. Use `theme-deploy` skill to deploy — user imports content separately on live site
