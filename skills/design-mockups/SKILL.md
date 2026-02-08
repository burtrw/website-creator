---
name: design-mockups
description: "Build and present HTML/CSS design mockups with a local preview server. Use when prototyping website designs, iterating on visual concepts, or presenting design options."
---

# Design Mockups Skill

Create HTML/CSS design mockups and present them to users for review.

## When to Use

Activate this skill when:
- User wants to see design concepts before building a theme
- Creating multiple style explorations (terminal, editorial, minimal, etc.)
- Iterating on visual direction for a website

---

## CRITICAL: Image Color Cohesion

**Hero and prominent images MUST incorporate the site's color palette.**

Random stock photos look disconnected and amateurish. For a polished, professional feel:

### Image Sourcing Options (in order of preference)

1. **Unsplash → Midjourney edit** — Find an Unsplash image with the right composition, then use Midjourney to edit/restyle it with brand colors
2. **Generate from scratch** — Use DALL-E/Midjourney specifying exact hex colors from the palette
3. **Edit in image tool** — Add duotone effects, color overlays, or gradient washes in Photoshop/Figma
4. **CSS overlays (fallback)** — Apply gradient overlays on hero images:
   ```css
   .hero::after {
       content: '';
       position: absolute;
       inset: 0;
       background: linear-gradient(135deg, rgba(PRIMARY, 0.3), rgba(SECONDARY, 0.2));
   }
   ```

### Examples of Color Integration
- Button accent color appears subtly in hero image lighting/objects
- Background section color echoed in product photography
- Brand green reflected in landscape/nature imagery tones

**The goal:** Images should feel *designed for this site*, not grabbed from a stock library.

---

## CRITICAL: Contrast Verification

**Run this check on every color palette BEFORE creating mockups:**

```python
def check_contrast(hex1, hex2):
    """Returns contrast ratio between two hex colors."""
    def hex_to_rgb(h):
        h = h.lstrip('#')
        return tuple(int(h[i:i+2], 16) for i in (0, 2, 4))

    def luminance(rgb):
        r, g, b = [x/255 for x in rgb]
        r = r/12.92 if r <= 0.03928 else ((r+0.055)/1.055)**2.4
        g = g/12.92 if g <= 0.03928 else ((g+0.055)/1.055)**2.4
        b = b/12.92 if b <= 0.03928 else ((b+0.055)/1.055)**2.4
        return 0.2126*r + 0.7152*g + 0.0722*b

    l1, l2 = luminance(hex_to_rgb(hex1)), luminance(hex_to_rgb(hex2))
    return (max(l1,l2) + 0.05) / (min(l1,l2) + 0.05)

# UPDATE THESE WITH YOUR PALETTE
palette = {
    'bg': '#FFFFFF',
    'text': '#1a1a1a',
    'muted': '#666666',
    'primary': '#BF5700',
}

# Check all text/background combinations
for name, color in palette.items():
    if name != 'bg':
        ratio = check_contrast(color, palette['bg'])
        status = "PASS" if ratio >= 4.5 else "FAIL"
        print(f"{name} on bg: {ratio:.1f}:1 [{status}]")
```

**Requirements:**
- Normal text: **4.5:1** minimum
- Large text (18pt+): **3:1** minimum
- **DO NOT create mockups with FAIL results — fix the palette first**

---

## Project Structure

```
{project-name}/
├── mockups/
│   ├── concept-a.html       # Style exploration A
│   ├── concept-b.html       # Style exploration B
│   ├── concept-c.html       # Style exploration C
│   └── {chosen-direction}/  # After selection: full templates
│       ├── styles.css
│       ├── home.html
│       ├── post.html
│       └── about.html
├── project-brief.json       # Requirements
└── README.md
```

## CRITICAL: Presenting Mockups to Users

### DO NOT use localhost servers

```javascript
// DON'T DO THIS - servers don't work in Cowork VM
app.listen(3000, () => console.log('http://localhost:3000'));
```

**Why it fails:** The Cowork VM runs in an isolated environment. Local servers are not accessible from the user's browser.

### DO use computer:// links

Always present mockups using `computer://` protocol links:

```markdown
| Concept | Link |
|---------|------|
| **Terminal Style** | [View Mockup](computer:///path/to/mockups/terminal.html) |
| **Editorial Style** | [View Mockup](computer:///path/to/mockups/editorial.html) |
| **Minimal Style** | [View Mockup](computer:///path/to/mockups/minimal.html) |
```

### Template for Presenting Mockups

After creating mockups, present them like this:

```markdown
Done! **[N] design concepts** are ready:

| Concept | Description | Link |
|---------|-------------|------|
| **[Name 1]** | [Brief description of style/vibe] | [View Mockup](computer:///full/path/to/mockup1.html) |
| **[Name 2]** | [Brief description of style/vibe] | [View Mockup](computer:///full/path/to/mockup2.html) |
| **[Name 3]** | [Brief description of style/vibe] | [View Mockup](computer:///full/path/to/mockup3.html) |

**Pick your favorite** (or tell me what to mix/change), and I'll build the full WordPress theme!
```

### Path Format

- Always use **absolute paths** starting from `/sessions/`
- The full format is: `computer:///sessions/happy-zen-knuth/mnt/Desktop/project-name/mockups/file.html`
- Or if in a user-mounted folder: `computer:///sessions/happy-zen-knuth/mnt/[mounted-folder]/...`

## Creating Mockups

### Self-Contained HTML

Each mockup should be a single HTML file with embedded CSS:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Project Name] — [Concept Name]</title>
    <link href="https://fonts.googleapis.com/css2?family=..." rel="stylesheet">
    <style>
        /* All styles embedded */
        :root {
            --primary: #BF5700;
            --bg: #ffffff;
            --text: #1a1a1a;
        }
        /* ... rest of styles */
    </style>
</head>
<body>
    <!-- Full page mockup content -->
</body>
</html>
```

### Best Practices

1. **Embed everything** — No external CSS files (Google Fonts URLs are OK)
2. **Show the full vibe** — Include enough content to judge the aesthetic
3. **Use real-ish content** — Avoid lorem ipsum when possible
4. **Mobile responsive** — Test at mobile widths
5. **4-5 key colors** — Define a clear palette in CSS variables
6. **No orphans** — Use text-wrap to prevent single words on their own line

### Typography Rules

Always include these text-wrap rules to prevent ugly orphan words:

```css
h1, h2, h3, h4, h5, h6 {
    text-wrap: balance;  /* Balances line lengths in headings */
}
p {
    text-wrap: pretty;   /* Prevents orphan words in paragraphs */
}
```

### Color Palette Convention

**Run the contrast check script (see CRITICAL section above) before using any palette.**

```css
:root {
    --bg: #ffffff;           /* Background */
    --text: #1a1a1a;         /* Primary text */
    --muted: #666666;        /* Secondary text */
    --primary: #BF5700;      /* Brand/accent color */
    --primary-light: #FFE8D6; /* Light variant */
}
```

## Design Phase Workflow

### Phase 1: Style Exploration
Create 3-4 distinct visual directions. Each mockup shows a complete aesthetic.

**Location:** `mockups/*.html` (root level)

### Phase 2: User Selection
Present mockups using computer:// links. User picks a direction.

### Phase 3: Full Templates
After selection, build out all page types in chosen style.

**Location:** `mockups/{chosen-direction}/`

```
mockups/
└── terminal-v2/
    ├── styles.css    # Shared design system
    ├── home.html
    ├── post.html
    ├── page.html
    ├── archive.html
    └── 404.html
```

## Naming Conventions

| Type | Pattern | Example |
|------|---------|---------|
| Style exploration | `{style-name}.html` | `open-terminal.html` |
| Style iteration | `{style-name}-v2.html` | `terminal-v2.html` |
| Page template | `{page-type}.html` | `post.html` |

## Transition to Theme Building

When user selects a design:

```markdown
Great choice! I'll now convert the **[Selected Style]** mockup into a full WordPress block theme.

This will include:
- theme.json with your colors, fonts, and spacing
- Templates for all page types
- Block patterns for key sections
- WordPress Playground preview

Building your theme...
```

Then invoke the `wp-block-themes` skill.
