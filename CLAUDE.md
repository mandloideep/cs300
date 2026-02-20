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

Common icons: `code`, `document-text`, `document`, `academic-cap`, `terminal`, `color-swatch`, `sparkles`, `cog`, `globe-alt`, `view-boards`, `view-grid`

## Checklist: Adding a New Session

When adding a new session to an existing week, follow these steps in order:

1. **Create the session file** at `content/notes/week-XX/session-X.md` using the session template above
2. **Update the week's `_index.md`** to add/update the session card (change "Coming soon" to actual subtitle)
3. **Update `content/notes/_index.md`** if this is the first session for a new week (change week card from "Coming soon")

## Checklist: Adding a New Week

When adding content for a completely new week, follow these steps in order:

1. **Create the week folder** at `content/notes/week-XX/`
2. **Create the week `_index.md`** using the week template above
3. **Create session files** (`session-1.md`, `session-2.md`, etc.) using the session template
4. **Update `content/notes/_index.md`** to update the week card from "Coming soon" to the actual title, icon, and subtitle

## Pending Content Releases

These files exist locally but are gitignored until ready to publish. When releasing each item:
1. Remove the file from `.gitignore`
2. Add a card for it in the corresponding section `_index.md`
3. Commit both changes and push

- [ ] **Assignment 3** (`content/notes/assignments/assignment-3.md`) — add card to `content/notes/assignments/_index.md`
- [ ] **Assignment 4** (`content/notes/assignments/assignment-4.md`) — add card to `content/notes/assignments/_index.md`
- [ ] **Group Project 2** (`content/notes/group-projects/group-project-2.md`) — add card to `content/notes/group-projects/_index.md`
- [ ] **Final Project** (`content/notes/individual-projects/final-project.md`) — add card to `content/notes/individual-projects/_index.md`
