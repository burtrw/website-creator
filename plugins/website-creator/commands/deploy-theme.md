# /deploy-theme

Deploy a finished WordPress theme to live hosting.

## Prerequisites

A completed theme project with:
- `theme.zip` - The packaged theme
- `blueprint.json` - Playground blueprint (optional)
- `open-in-playground.html` - Preview page (optional)

## Workflow

Uses the `theme-deploy` skill:

1. **Verify theme is ready** - Check theme.zip exists and contains required files
2. **Search current pricing** - Always use WebSearch for WordPress.com plans
3. **Present options** - WordPress.com (recommended) + download/self-host
4. **Guide deployment** - Based on user preference

## Usage

```
User: /deploy-theme

Claude: [Searches for current WordPress.com pricing]
Claude: [Presents hosting options with current prices]

User: I want to use WordPress.com

Claude: [Guides through WordPress.com setup]
```

## Important Rules

- **Always search for current pricing** - WordPress.com plans change frequently
- **Never assume prices** - Training data may be outdated
- **Always offer alternatives** - Some users prefer self-hosting
