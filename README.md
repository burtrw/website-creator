# Website Creator Plugin

A Cowork plugin that creates websites from simple descriptions with live WordPress Playground preview.

## Commands

| Command | Description |
|---------|-------------|
| `/create-theme` | Main workflow: describe your site, review mockups, generate theme, preview and deploy |
| `/deploy-theme` | Deploy a finished theme to WordPress.com or download for self-hosting |

## Skills

| Skill | Description |
|-------|-------------|
| `wp-block-themes` | WordPress FSE theme architecture, theme.json, templates, patterns, design systems |
| `design-mockups` | Create and present HTML/CSS design mockups |
| `theme-deploy` | Deploy themes to WordPress.com or export for self-hosting |

## Example Workflow

```
> /create-theme I want a playful site for my digital agency called Craft Digital

[Claude asks clarifying questions about site type, tone, logo]

[Claude generates 3-4 design mockups as HTML files]

> I like the Geometric Pop design!

[Claude builds full WordPress block theme]
[Creates WordPress Playground preview]

> I love it - how do I get this live?

[Claude searches for current WordPress.com pricing]
[Offers WordPress.com deployment or theme download]
```

## What Gets Generated

```
project-name/
├── mockups/
│   ├── concept-a.html
│   ├── concept-b.html
│   └── concept-c.html
├── theme/
│   ├── theme.json
│   ├── style.css
│   ├── functions.php
│   ├── screenshot.png
│   ├── templates/
│   ├── parts/
│   └── patterns/
├── theme.zip
├── blueprint.json
└── open-in-playground.html
```

## Key Features

### Design Systems (Anti-AI-Slop)
The `wp-block-themes` skill includes guidance on:
- 8 distinct design directions (minimal, maximalist, retro-futuristic, etc.)
- Typography recommendations by site type
- Color proportions (60/30/10 rule)
- Font anti-patterns (avoiding overused AI fonts)

### Always-Current Pricing
The `theme-deploy` skill **always searches** for current WordPress.com pricing rather than relying on training data. Plan names and prices change frequently.

### WordPress Playground Preview
Themes are bundled into blueprints that can be instantly previewed in WordPress Playground without any server setup.

## Installation

### Cowork
Zip this folder and upload to Cowork, or add via plugin settings.

### Claude Code
```bash
claude plugins add /path/to/website-creator
```

## Plugin Structure

```
website-creator/
├── plugin.json
├── README.md
├── commands/
│   ├── create-theme.md
│   └── deploy-theme.md
└── skills/
    ├── wp-block-themes/
    │   └── SKILL.md
    ├── design-mockups/
    │   └── SKILL.md
    └── theme-deploy/
        └── SKILL.md
```
