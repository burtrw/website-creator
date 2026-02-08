---
name: content-import
description: "Import existing website content for redesigns. Use when redesigning an existing site. Triggers: 'import content', 'existing site', 'WordPress export', 'XML import', 'migrate content', 'redesign my site', 'my current site is...'."
---

# Content Import Skill

Import existing content when redesigning a site.

## When to Use

- User is redesigning an existing site
- User has an export file (WordPress XML preferred)
- User provides a URL to their existing site

## Content Sources (in order of preference)

### 1. WordPress XML Export (Preferred)

The most complete option — includes posts, pages, categories, tags, menus, and metadata.

**How to get it:** Tools → Export in WordPress admin (or Settings → Export on WordPress.com)

```python
import xml.etree.ElementTree as ET
from html import unescape

def parse_wordpress_xml(xml_path):
    """Parse WordPress XML export file."""
    tree = ET.parse(xml_path)
    root = tree.getroot()

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

    channel = root.find('channel')
    content['site_title'] = channel.findtext('title', '')
    content['site_description'] = channel.findtext('description', '')

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

    for cat in channel.findall('wp:category', ns):
        content['categories'].append({
            'name': cat.findtext('wp:cat_name', '', ns),
            'slug': cat.findtext('wp:category_nicename', '', ns),
        })

    return content
```

### 2. Scrape Existing Site (No Export File)

If user doesn't have an export file but provides their site URL, scrape key pages:

```python
def scrape_existing_site(base_url):
    """Scrape starter content from an existing site."""
    from urllib.request import urlopen
    from html.parser import HTMLParser
    import re

    content = {
        'site_title': '',
        'site_description': '',
        'pages': [],
        'navigation': [],
    }

    # Pages to attempt scraping
    key_pages = [
        ('/', 'Home'),
        ('/about', 'About'),
        ('/about-us', 'About'),
        ('/services', 'Services'),
        ('/contact', 'Contact'),
        ('/contact-us', 'Contact'),
    ]

    for path, default_title in key_pages:
        try:
            url = base_url.rstrip('/') + path
            # Use WebFetch tool to get page content
            # Extract: title, main heading, body text, images
            page_content = fetch_and_parse(url)
            if page_content:
                content['pages'].append({
                    'title': page_content.get('title', default_title),
                    'slug': path.strip('/') or 'home',
                    'content': page_content.get('main_content', ''),
                    'headings': page_content.get('headings', []),
                })
        except:
            continue

    # Try to extract navigation structure from homepage
    try:
        nav = extract_navigation(base_url)
        content['navigation'] = nav
    except:
        pass

    return content
```

**What to extract when scraping:**
- Site title and tagline (from `<title>` and meta description)
- Navigation menu structure (from `<nav>` elements)
- Homepage hero text and CTA
- About page content
- Services/offerings list
- Contact information
- Key headings and body copy

### 3. Other Import Formats

Support other formats when provided:
- **JSON** — Parse and map to content structure
- **CSV** — For posts/products with title, content, category columns
- **Markdown files** — Convert to WordPress content
- **HTML files** — Extract content from static site

## Workflow

### Step 1: Gather Content

Ask user:
1. Do you have a WordPress XML export file?
2. If not, what's the URL of your existing site?

### Step 2: Parse/Scrape and Summarize

```markdown
## Content Found

**Site:** [Site Title]
**Tagline:** [Description]

| Type | Count |
|------|-------|
| Pages | X |
| Posts | X |
| Menu Items | X |

### Pages
- Home — [brief description of content]
- About — [brief description]
- Services — [brief description]
- Contact — [brief description]

### Navigation Structure
- Home
- About
- Services
  - Service A
  - Service B
- Contact
```

### Step 3: Use Content to Inform Design

**DO NOT add content to the blueprint** — it bloats the file size.

Instead, use imported content to:
- **Inform navigation structure** — Build header/menu based on actual pages
- **Match content length** — Design layouts that fit real content (not lorem ipsum)
- **Identify key sections** — Hero text, CTAs, service offerings
- **Set site title/tagline** — Use in mockups and theme preview

The Playground preview shows the theme structure with placeholder content. The user imports their real content after deploying to their live site.

## Important Notes

1. **Content informs design, not blueprint** — Don't inject content into Playground (bloats file size)
2. **Templates use dynamic blocks** — Theme pulls content from database, not hardcoded
3. **User imports separately** — After deploying theme, user imports XML on live site
4. **Preserve structure** — Use navigation structure to inform header/menu patterns

## Integration with Other Skills

1. Import content first
2. Use `design-mockups` skill informed by actual content structure
3. Use `wp-block-themes` skill to build theme with dynamic templates
4. Use `theme-deploy` skill — user imports content separately on live site
