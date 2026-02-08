---
name: theme-deploy
description: "Deploy a finished WordPress theme to hosting. Use when the user is happy with their theme and wants to launch it. Triggers: 'deploy', 'launch', 'go live', 'publish site', 'host this', 'put this online', 'how do I use this theme', 'get this on WordPress.com'. Recommends WordPress.com (with current pricing) but also offers self-hosted export options."
---

# Theme Deploy

Deploy finished WordPress themes to live hosting.

## Workflow

### Step 1: Confirm Theme is Ready

Verify the theme package exists:
- `theme.zip` in project directory
- Contains: style.css, theme.json, templates/, parts/

If missing, build the theme package first.

### Step 2: Search for Current Pricing

**CRITICAL: Never rely on training data for WordPress.com pricing.**

Always search first:
```
WebSearch: "WordPress.com plans pricing custom themes [current year]"
```

WordPress.com pricing and plan names change. Search every time.

### Step 3: Present Options

#### Option A: WordPress.com (Recommended)

Present current pricing from search results. Typical benefits:
- Managed hosting (no server maintenance)
- Automatic updates and security
- Built-in backups, CDN, SSL

Deployment steps:
1. Create site at wordpress.com/start
2. Choose paid plan supporting custom themes
3. Appearance → Themes → Upload
4. Upload theme.zip and activate

#### Option B: Download for Self-Hosting

For WP Engine, Bluehost, self-managed, etc:

Provide download links:
- theme.zip (the theme)
- blueprint.json (for Playground testing)
- open-in-playground.html (preview page)

Installation methods:
- Admin: Appearance → Themes → Add New → Upload
- FTP: Upload to `/wp-content/themes/`
- WP-CLI: `wp theme install theme.zip --activate`

### Step 4: Ask User Preference

```
Question: "How would you like to deploy?"
Options:
- WordPress.com (Recommended) - Managed hosting, easiest setup
- Download theme.zip - Take it to any WordPress host
- Both
```

## Response Template

```markdown
Your **[Theme Name]** is ready to go live!

### WordPress.com (Recommended)
[Insert current pricing from search]

1. Go to [wordpress.com/start](https://wordpress.com/start)
2. Choose [plan] or higher
3. Upload at Appearance → Themes → Upload

### Download & Self-Host
[Download theme.zip](computer:///path/to/theme.zip)

Install via WordPress admin or WP-CLI.

Which would you prefer?
```

## Rules

- **Always search for current pricing** - Never assume plan names or prices
- **Always offer download option** - Don't lock users into WordPress.com
- **Keep it simple** - Don't overwhelm with hosting comparisons
