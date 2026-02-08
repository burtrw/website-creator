---
name: wp-block-themes
description: "Use when developing WordPress block themes - theme.json (global settings/styles), templates and template parts, patterns, style variations, and Site Editor troubleshooting. Triggers include theme.json editing, block theme templates, WordPress patterns, style variations, and debugging styles not applying issues."
---

# WP Block Themes Skill

## Absolute Rules
- **NO EMOJIS**: Never use emojis anywhere in generated content - not in headings, paragraphs, button text, or any other text. This applies to all templates, patterns, and content.
- **NO ORPHANS**: Always use CSS text-wrap to prevent single words on their own line:
  ```css
  h1, h2, h3, h4, h5, h6 {
    text-wrap: balance;  /* Balances line lengths in headings */
  }
  p {
    text-wrap: pretty;   /* Prevents orphan words in paragraphs */
  }
  ```

---

## Design Phase Questions

**IMPORTANT:** Before starting any theme work, gather this information:

### Required Questions
1. **Site Name** - What's the site/business called?
2. **Site Type** - SaaS, portfolio, blog, restaurant, e-commerce, agency, etc.?
3. **Logo** - Does a logo exist, should we create one, or use text-only site title?
4. **Primary Goal** - What action should visitors take? (signup, buy, contact, read)
5. **Tone/Feel** - Professional, playful, minimal, bold, luxurious, editorial?

### Optional Questions
- Target audience description
- Brand colors (if established)
- Inspiration sites or design references
- Required pages/sections (hero, features, testimonials, pricing, etc.)

---

## Site Specification Schema

Use this structure when planning themes:

```json
{
  "siteBrief": {
    "siteName": "Name of site/business",
    "siteType": "SaaS | portfolio | blog | restaurant | e-commerce",
    "primaryGoal": "Main conversion goal",
    "audience": "Target audience description",
    "tone": "Voice and feel",
    "brandKeywords": "Aesthetic descriptors"
  },
  "layoutNotes": [
    "Hero with value prop and CTA",
    "Feature grid or comparison",
    "Social proof (testimonials, logos)",
    "Final CTA"
  ],
  "typography": {
    "primaryFont": "Heading font",
    "secondaryFont": "Body font",
    "fontImport": "Google Fonts URL"
  }
}
```

Present specs as a table for user confirmation:
| Field | Value |
|-------|-------|
| Site Name | [name] |
| Site Type | [type] |
| Primary Goal | [goal] |
| Tone | [tone] |
| Key Sections | [list] |

---

## Design Systems (Avoiding "AI Slop")

### Pick ONE Bold Direction
Generic designs fail because they try to be everything. Commit fully to a direction:

| Direction | Characteristics |
|-----------|----------------|
| Brutally minimal | Maximum whitespace, single accent color, stark typography |
| Maximalist | Dense information, layered elements, controlled complexity |
| Retro-futuristic | Vintage meets tech, neon on dark, geometric shapes |
| Organic/natural | Soft curves, earthy tones, textured backgrounds |
| Luxury/refined | Rich colors, sophisticated serif typography, premium feel |
| Playful | Bold colors, rounded shapes, unexpected interactions |
| Editorial/magazine | Grid-based, strong typographic hierarchy, clean sections |
| Brutalist/raw | Unconventional choices, exposed structure, monospace fonts |

### Typography Anti-Patterns

**AVOID (overused/generic):**
- Inter, Roboto, Arial, system fonts
- Space Grotesk (overused by AI)
- Generic blue (#007bff) as primary color
- Purple gradients on white (cliché AI aesthetic)

**PREFER (distinctive choices):**
- Fraunces, Clash Display, Cabinet Grotesk, Satoshi, Outfit
- Syne, DM Serif Display, Playfair Display, Cormorant Garamond
- Match font personality to brand

### Font Recommendations by Site Type

| Site Type | Heading Font | Body Font |
|-----------|-------------|-----------|
| SaaS/Tech | Satoshi, Plus Jakarta Sans, Outfit | Inter, Source Sans Pro |
| Law/Finance | Cormorant Garamond, DM Serif Display | Source Sans Pro |
| Restaurant | Playfair Display, Lora | Josefin Sans, Lato |
| Creative/Portfolio | Clash Display, Fraunces, Syne | Inter, Work Sans |
| Blog/Media | Outfit, Merriweather | Source Serif Pro |
| E-commerce (luxury) | Cormorant, Playfair | Lato, Open Sans |
| E-commerce (modern) | Outfit, Satoshi | Inter |

### Color Proportions
- **60%** dominant (backgrounds, large areas)
- **30%** secondary (containers, sections)
- **10%** accent (CTAs, highlights)

---

## Layout Inference by Site Type

When a user describes their site, infer the typical sections and structure:

### SaaS / Technology
- **Goal:** Drive signups or demo requests
- **Tone:** Professional, innovative, trustworthy
- **Typical layout:**
  - Hero with value proposition and CTA
  - Feature grid or comparison
  - Social proof (logos, testimonials)
  - Pricing section
  - FAQ
  - Final CTA

### Restaurant / Food Service
- **Goal:** Drive reservations or visits
- **Tone:** Warm, inviting, appetite-inducing
- **Typical layout:**
  - Hero with appetizing food imagery
  - Menu sections
  - About/story
  - Location and hours
  - Reservation CTA
  - Instagram or gallery

### Professional Services (Law, Finance, Consulting)
- **Goal:** Generate inquiries, establish credibility
- **Tone:** Professional, authoritative, trustworthy
- **Typical layout:**
  - Hero with credentials or value statement
  - Services/practice areas
  - Team profiles
  - Case studies or results
  - Client testimonials
  - Contact/consultation CTA

### Creative / Portfolio
- **Goal:** Showcase work, attract clients
- **Tone:** Creative, distinctive, personality-driven
- **Typical layout:**
  - Full-bleed portfolio hero
  - Project gallery or case studies
  - About/bio section
  - Services offered
  - Contact or inquiry form

### Blog / Media
- **Goal:** Engage readers, build audience
- **Tone:** Varies by niche (authoritative, casual, entertaining)
- **Typical layout:**
  - Featured post hero
  - Recent posts grid or list
  - Category navigation
  - About the author
  - Newsletter signup
  - Popular/trending section

### E-commerce / Retail
- **Goal:** Drive purchases
- **Tone:** Varies by brand positioning
- **Typical layout:**
  - Hero with featured products or promotion
  - Category navigation
  - Featured products grid
  - Trust signals (reviews, guarantees)
  - Newsletter signup

---

### Motion & Animation
Use animations for delight and micro-interactions:

```css
/* Staggered fade-in */
.fade-in {
  opacity: 0;
  transform: translateY(20px);
  animation: fadeIn 0.6s ease forwards;
}
.fade-in:nth-child(1) { animation-delay: 0.1s; }
.fade-in:nth-child(2) { animation-delay: 0.2s; }
.fade-in:nth-child(3) { animation-delay: 0.3s; }

@keyframes fadeIn {
  to { opacity: 1; transform: translateY(0); }
}

/* Smooth hover transitions */
.interactive {
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}
.interactive:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.15);
}
```

---

## Inputs Required
- Repository root and target theme directory (if multiple themes exist)
- Target WordPress version range (theme.json schema varies by core version)
- Issue manifestation location: Site Editor, post editor, frontend, or multiple areas

## Procedure

### Step 0: Triage and Locate Block Theme Roots
1. Detect theme roots by looking for `theme.json` files
2. If multiple themes exist, scope all changes to one theme root

### Step 1: Create New Block Theme (if needed)
- Prefer starting from known-good scaffolds or WP exports rather than guessing layouts
- Be explicit about minimum supported WordPress version
- Block theme minimum structure:
  - `style.css` (with theme header)
  - `theme.json`
  - `templates/` directory with at least `index.html`

### Step 2: Confirm Theme Type and Override Expectations
Block theme indicators:
- `theme.json` present
- `templates/` and/or `parts/` directories present

**Style hierarchy to remember:**
Core defaults → theme.json → child theme → user customizations

User customizations can make theme.json edits appear "ignored" - check the database for overrides.

### Step 3: Make theme.json Changes Safely
Decide whether changing:
- **Settings** (UI capabilities): presets, typography scale, colors, layout, spacing
- **Styles** (default appearance): CSS-like rules for elements/blocks

Key theme.json sections:
```json
{
  "$schema": "https://schemas.wp.org/trunk/theme.json",
  "version": 3,
  "settings": {
    "color": { "palette": [...] },
    "typography": { "fontSizes": [...] },
    "layout": { "contentSize": "800px", "wideSize": "1200px" }
  },
  "styles": {
    "color": { "background": "...", "text": "..." },
    "typography": { "fontSize": "...", "fontFamily": "..." },
    "elements": { "link": {...}, "button": {...} },
    "blocks": { "core/paragraph": {...} }
  }
}
```

### Step 4: Templates and Template Parts
- Templates reside in `templates/` as HTML files (e.g., `index.html`, `single.html`, `page.html`)
- Template parts must live in `parts/` without nested subdirectories (e.g., `header.html`, `footer.html`)
- Use block markup: `<!-- wp:template-part {"slug":"header"} /-->`

**Required templates for all themes:**
- `page.html` - Page with title
- `page-no-title.html` - Page without title (for landing pages)
- `single.html` - Blog post template with: Categories, Date, Title, Featured Image, Comments
- `index.html` - Blog archive/fallback
- `front-page.html` - Homepage (if applicable)
- `404.html` - Error page

**Single post template should include (in order):**
```html
<!-- wp:post-terms {"term":"category"} /-->
<!-- wp:post-date /-->
<!-- wp:post-title {"level":1} /-->
<!-- wp:post-featured-image {"aspectRatio":"16/9"} /-->
<!-- wp:post-content /-->
<!-- wp:comments -->
<!-- Full comments section with comment-template, pagination, post-comments-form -->
<!-- /wp:comments -->
```

### Step 5: Header Best Practices

**IMPORTANT: Site Title block should NOT be an H1**
- Always use the Site Title block: `<!-- wp:site-title {"level":0} /-->`
- The `"level":0` attribute renders as a `<p>` tag instead of a heading
- Never omit the level attribute or set it to 1, as that creates an H1
- H1 should be reserved for the main content heading (page title or post title)
- This improves SEO and accessibility - there should be one H1 per page describing the main content

**Header template part example:**
```html
<!-- wp:group {"tagName":"header"} -->
<header class="wp-block-group">
  <!-- wp:group {"layout":{"type":"flex","justifyContent":"space-between"}} -->
  <div class="wp-block-group">
    <!-- wp:site-title {"level":0} /-->
    <!-- wp:navigation /-->
  </div>
  <!-- /wp:group -->
</header>
<!-- /wp:group -->
```

### Step 6: Patterns
Prefer filesystem patterns under `patterns/` for theme-owned patterns.

Pattern file header example:
```php
<?php
/**
 * Title: Hero Section
 * Slug: theme-name/hero
 * Categories: featured
 */
?>
<!-- wp:cover {"...} -->
```

### Step 7: Style Variations
Style variations are JSON files under `styles/` directory.

Note: Once users select a variation, it's stored in the database, so file changes may not auto-update their view.

Example: `styles/dark.json`
```json
{
  "$schema": "https://schemas.wp.org/trunk/theme.json",
  "version": 3,
  "title": "Dark",
  "styles": {
    "color": {
      "background": "#1a1a1a",
      "text": "#ffffff"
    }
  }
}
```

## Verification
- Site Editor reflects changes in expected locations (Styles UI, templates, patterns)
- Frontend renders with expected styles
- If styles aren't changing, confirm whether user customizations override theme defaults
- Run repository build/lint scripts if assets are involved (fonts, custom JS/CSS)
- Check that H1 only appears once per page (in main content, not header)

## Visual Testing Before Sharing

**CRITICAL:** Before sharing a theme preview with users, ALWAYS compare the live theme against the original mockups/designs.

### Design Fidelity Checklist
When building from mockups, verify these CSS features are implemented:

1. **Background effects**
   - [ ] Background patterns (e.g., graph paper, gradients)
   - [ ] Background colors and images

2. **Hover animations**
   - [ ] Navigation link hover effects (underlines, color changes)
   - [ ] Button hover states
   - [ ] Card/post hover effects (transforms, shadows)

3. **Decorative elements**
   - [ ] Pseudo-element decorations (::before, ::after)
   - [ ] Circles, lines, borders on cards
   - [ ] Highlight effects on text

4. **Transitions and animations**
   - [ ] CSS transitions on interactive elements
   - [ ] Transform effects (translate, scale)
   - [ ] Box shadows on hover

### How to Verify
1. Read the mockup HTML/CSS to identify ALL custom styles
2. Cross-reference each style with the theme's style.css
3. Check that patterns/templates use the correct CSS classes
4. Test in WordPress Playground and visually compare to mockup

### ALWAYS Rebuild After Changes
**CRITICAL:** After ANY change to theme files, you MUST:
1. Rebuild the theme.zip
2. Regenerate blueprint.json with new base64
3. Regenerate open-in-playground.html

Use this single command after every change:
```bash
cd /path/to/project && rm -f theme.zip && cd theme && zip -r ../theme.zip . && cd .. && python3 << 'EOF'
import base64
import json

with open('theme.zip', 'rb') as f:
    theme_data = base64.b64encode(f.read()).decode('utf-8')

# Load existing blueprint or create new one
# ... update with new theme_data ...
# Write blueprint.json and open-in-playground.html
EOF
```

**Never share a playground link without rebuilding first!**

### Common Missed Items
- Nav link hover animations (::after pseudo-elements)
- Post card decorative circles (::before)
- Text highlight effects (::after with background)
- Background patterns on body
- Transition timing functions

## Debugging Common Issues
1. **Wrong theme root** - Verify you're editing the active theme
2. **User customizations override defaults** - Check `wp_global_styles` post type in database
3. **Invalid theme.json** - Validate JSON syntax and schema version
4. **Templates/parts in wrong folders** - Parts must be in `parts/`, not nested
5. **Cache issues** - Clear browser cache and any server-side caching
6. **Multiple H1s** - Site title in header using heading level causes SEO/a11y issues
7. **CSS not loading** - style.css must be enqueued in functions.php (see below)

### CRITICAL: Enqueue style.css in Block Themes
Block themes do NOT automatically load style.css. You MUST enqueue it:

```php
function theme_enqueue_styles() {
    wp_enqueue_style(
        'theme-style',
        get_stylesheet_uri(),
        array(),
        wp_get_theme()->get( 'Version' )
    );
}
add_action( 'wp_enqueue_scripts', 'theme_enqueue_styles' );
add_action( 'enqueue_block_assets', 'theme_enqueue_styles' );
```

Without this, all custom CSS in style.css will be ignored!

## WordPress Playground Blueprints

When creating WordPress Playground blueprints for theme previews:

### Zip Structure
**CRITICAL:** Theme files must be at the ROOT of the zip, not nested in a folder.

```bash
# WRONG - creates theme/style.css in zip
zip -r theme.zip theme/

# CORRECT - creates style.css at root
cd theme && zip -r ../theme.zip .
```

If you get "Stylesheet is missing" error, the zip structure is wrong.

### Writing Large Files (Base64 Data)
**IMPORTANT:** The Write tool times out with large strings (~20KB+). Always use Python to write files containing base64-encoded theme data.

### Complete Blueprint Encoding Recipe

Use this Python script to build the entire preview package:

```python
python3 << 'PYTHON_EOF'
import base64
import json
import os

THEME_NAME = "theme-name"  # Change this
PROJECT_DIR = "/path/to/project"  # Change this

os.chdir(PROJECT_DIR)

# 1. Base64 encode the theme.zip
with open('theme.zip', 'rb') as f:
    theme_data = base64.b64encode(f.read()).decode('utf-8')

# 2. Build the blueprint with inline data URL
blueprint = {
    "$schema": "https://playground.wordpress.net/blueprint-schema.json",
    "landingPage": "/",
    "preferredVersions": {
        "php": "8.0",
        "wp": "latest"
    },
    "steps": [
        {
            "step": "writeFile",
            "path": f"/wordpress/wp-content/themes/{THEME_NAME}.zip",
            "data": {
                "resource": "url",
                "url": f"data:application/zip;base64,{theme_data}"
            }
        },
        {
            "step": "unzip",
            "zipPath": f"/wordpress/wp-content/themes/{THEME_NAME}.zip",
            "extractToPath": f"/wordpress/wp-content/themes/{THEME_NAME}"
        },
        {
            "step": "activateTheme",
            "themeFolderName": THEME_NAME
        }
    ]
}

# 3. Save blueprint.json
with open('blueprint.json', 'w') as f:
    json.dump(blueprint, f, indent=2)

# 4. Encode for URL - CRITICAL: use compact JSON, minimal escaping
blueprint_compact = json.dumps(blueprint, separators=(',', ':'))
encoded_blueprint = blueprint_compact.replace(' ', '%20').replace('"', '%22')
playground_url = f"https://playground.wordpress.net/#{encoded_blueprint}"

# 5. Generate open-in-playground.html with theme styling
html = f'''<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{THEME_NAME} Theme Preview</title>
    <style>
        body {{ font-family: system-ui, sans-serif; max-width: 800px; margin: 4rem auto; padding: 2rem; }}
        h1 {{ margin-bottom: 0.5rem; }}
        .btn {{ display: inline-block; padding: 1rem 2rem; background: #007cba; color: white; text-decoration: none; border-radius: 4px; margin: 0.5rem 0.5rem 0.5rem 0; }}
        .btn:hover {{ background: #005a87; }}
        .btn.secondary {{ background: #50575e; }}
    </style>
</head>
<body>
    <h1>{THEME_NAME}</h1>
    <p>WordPress Block Theme</p>

    <h2>Preview</h2>
    <a href="{playground_url}" class="btn" target="_blank">Open in WordPress Playground</a>

    <h2>Download</h2>
    <a href="data:application/zip;base64,{theme_data}" download="{THEME_NAME}.zip" class="btn secondary">Download theme.zip</a>
</body>
</html>'''

with open('open-in-playground.html', 'w') as f:
    f.write(html)

print(f"Created: blueprint.json, open-in-playground.html")
print(f"Theme data size: {len(theme_data)} chars")
PYTHON_EOF
```

**URL Encoding Rules:**
- Use **compact JSON**: `json.dumps(blueprint, separators=(',', ':'))`
- Only escape quotes and spaces: `.replace(' ', '%20').replace('"', '%22')`
- Do NOT use `urllib.parse.quote()` — it over-encodes and breaks Playground

### Sharing Playground Preview with Users

**Workflow:**
1. Create `open-in-playground.html` with the blueprint embedded in the URL hash
2. Give user a `computer://` link to open the HTML file
3. User clicks "Open in Chrome" button to launch Playground

**IMPORTANT:** The `computer://` protocol only works for files, not directories.

**Example response to user:**
```markdown
[Open Playground Preview](computer:///path/to/open-in-playground.html)
```

## Theme Screenshot Generation

**WordPress themes must have a screenshot.png** (1200x900 recommended) for display in the theme selector.

Use Pillow to generate a screenshot that represents the design.

## Image Handling

Use Unsplash for realistic placeholder images with sizing parameters:

```html
<!-- Hero background (full-width) -->
<img src="https://images.unsplash.com/photo-XXXX?w=1920&h=1080&fit=crop" alt="Hero background">
```

**Select contextually appropriate images:**
- Office/workspace for SaaS
- Food photography for restaurants
- Architecture for real estate
- Nature for wellness brands
- Abstract/geometric for tech

---

## Useful WP-CLI Commands
```bash
# Check active theme
wp theme list --status=active

# Export theme with customizations
wp theme export

# Clear transients/cache
wp transient delete --all
```
