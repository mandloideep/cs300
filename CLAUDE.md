# CS300 Course Site - Claude Guidelines

This Hugo site uses the **Hextra theme**. Follow these guidelines when adding or updating course content.

## Critical: Sidebar Configuration

The `content/notes/_index.md` file MUST have `cascade: type: docs` to enable the left sidebar navigation for all pages. Without this, pages will use the "default" layout which has no sidebar.

```yaml
# content/notes/_index.md front matter
---
title: "Course Notes"
weight: 1
cascade:
  type: docs
sidebar:
  open: true
---
```

## Adding New Week Content

When adding content for a new week (e.g., Week 2, Week 3, etc.):

### 1. Create the week folder structure

```
content/notes/week-XX/
├── _index.md      # Week overview with learning objectives
├── session-1.md   # First session content
└── session-2.md   # Second session content (if applicable)
```

### 2. Update `content/notes/_index.md`

Update the corresponding week card from "Coming soon" to actual content:

```markdown
{{< card link="week-XX" title="Week X: [Topic Name]" icon="document-text" subtitle="[Brief description of topics covered]" >}}
```

### 3. Week `_index.md` template

Use this template for new week overview pages. **Do NOT include a `# Title` heading** - the `title:` in front matter is rendered automatically.

```markdown
---
title: "Week X: [Topic Name]"
weight: X
sidebar:
  open: true
---

{{< callout type="info" >}}
**Learning Objectives:** [List what students will learn]
{{< /callout >}}

## Sessions

{{< cards >}}
  {{< card link="session-1" title="Session 1: [Title]" icon="code" subtitle="[Brief description]" >}}
  {{< card link="session-2" title="Session 2: [Title]" icon="academic-cap" subtitle="[Brief description]" >}}
{{< /cards >}}

## Topics Covered

- [Topic 1]
- [Topic 2]
- [etc.]
```

### 4. Session file template

**IMPORTANT:**
- Add `toc: true` to show the right sidebar (table of contents)
- **Do NOT include a `# Title` heading** - the `title:` in front matter is rendered automatically
- Start content with `## Section` headings

```markdown
---
title: "[Session Title]"
weight: X
toc: true
---

## Introduction

[Content here...]

## Section 2

[More content...]
```

**Note:** The right sidebar (table of contents) is generated from `##` headings in the content. Use proper heading hierarchy (`##` for main sections, `###` for subsections) to generate a useful table of contents.

## Hextra Shortcodes Available

- `{{< callout type="info|warning|error|important" >}}` - Highlighted info boxes
- `{{< cards >}}` with `{{< card >}}` - Card grids for navigation
- `{{< tabs >}}` with `{{< tab >}}` - Tabbed content
- `{{% steps %}}` - Numbered step lists
- `{{< details >}}` - Collapsible sections

## Icon Names for Cards

Common icons: `code`, `document-text`, `document`, `academic-cap`, `terminal`, `color-swatch`, `sparkles`, `cog`, `globe-alt`
