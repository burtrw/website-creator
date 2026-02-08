# /create-theme

Create a complete WordPress block theme from a description.

## Workflow

### Phase 0: Existing Content Check

**ALWAYS ask first:**
> Is this a new site or a redesign of an existing site?

If redesign:
1. Ask for WordPress XML export file (preferred), OR
2. Ask for the existing site URL to scrape key content

Use the `content-import` skill to:
- Parse XML export or scrape existing site
- Extract pages, posts, navigation structure
- Summarize content for user confirmation
- Inform design decisions (navigation, layout, content length)

**Note:** Content is NOT added to the blueprint (bloats file size). User imports content separately on live site.

### Phase 1: Discovery

Use the `wp-block-themes` skill's design phase questions:

1. **Site Name** - What's the site/business called?
2. **Site Type** - SaaS, portfolio, blog, restaurant, e-commerce, agency, etc.?
3. **Logo** - Does a logo exist, should we create one, or use text-only site title?
4. **Primary Goal** - What action should visitors take? (signup, buy, contact, read)
5. **Tone/Feel** - Professional, playful, minimal, bold, luxurious, editorial?

Present gathered info as a spec table for confirmation.

### Phase 2: Design Mockups

Use the `design-mockups` skill to create 3-4 HTML/CSS mockups.

- Create distinct visual directions based on spec
- Present with computer:// links
- Wait for user selection

### Phase 3: Build Theme

Use the `wp-block-themes` skill to convert the selected mockup:

1. Create theme.json with colors, fonts, spacing
2. Build templates (front-page, index, single, page, 404)
3. Create template parts (header, footer)
4. Add patterns for key sections
5. Generate screenshot.png
6. Build theme.zip
7. Create WordPress Playground preview

### Phase 4: Present Preview

Provide the user with:
- WordPress Playground preview link
- Theme download link
- Blueprint.json for testing

## Output Structure

```
{project-name}/
├── mockups/
│   ├── concept-a.html
│   ├── concept-b.html
│   └── concept-c.html
├── theme/
│   ├── style.css
│   ├── theme.json
│   ├── functions.php
│   ├── screenshot.png
│   ├── templates/
│   ├── parts/
│   └── patterns/
├── theme.zip
├── blueprint.json
└── open-in-playground.html
```

## Example Usage

### New Site
```
User: /create-theme I want a playful site for my digital agency called Craft Digital

Claude: Is this a new site or a redesign of an existing site?

User: New site

Claude: [Asks clarifying questions about site type, tone, logo]
Claude: [Creates 3-4 design mockups]

User: I like the Geometric Pop design!

Claude: [Builds full WordPress block theme]
Claude: [Creates Playground preview]
```

### Redesign
```
User: /create-theme I want to redesign mysite.com

Claude: Is this a new site or a redesign of an existing site?

User: Redesign

Claude: Do you have a WordPress XML export, or should I scrape the existing site?

User: Here's the XML [uploads file]

Claude: [Parses XML, summarizes content found]
Claude: Found 12 pages, 45 posts, navigation structure...
Claude: [Asks about desired new tone/feel]
Claude: [Creates mockups using real content]
```
