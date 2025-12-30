# STYLE_GUIDE - kiyânaw UI Conventions

## Two Card Types

### 1. `Theme.components.listItem` — Compact Lists
**When to use:** Any scrollable list of tappable items (questions, phrases, playlist items, responses)

| Property | Value |
|----------|-------|
| Padding | 16px (`md`) |
| Border radius | 8px (`md`) |
| Margin bottom | 8px (`sm`) |
| Title font | 16px, weight 500, lineHeight 22 |
| Meta font | 12px, secondary color |

### 2. `Theme.components.sectionCard` — Form Sections
**When to use:** Settings panels, form groups, profile sections

| Property | Value |
|----------|-------|
| Padding | 24px (`lg`) |
| Border radius | 16px (`lg`) |
| Margin bottom | 16px (`md`) |

---

## Container Padding

| Context | Horizontal Padding |
|---------|-------------------|
| List screens | 16px (`md`) |
| Form screens | 16px (`md`) |
| Modal content | 16px (`md`) |

---

## Typography Rules

| Element | Font Size | Weight |
|---------|-----------|--------|
| List item title | 16px | 500 |
| Meta/timestamps | 12px | normal |
| Section label | 14px | 600, uppercase |
| Page title | 20px | 600 |

**NEVER use `Theme.typography.h3` (20px) for list items — too large.**

---

## BANNED Patterns

- Blue left border on cards (`borderLeftWidth: 4`)
- `Theme.typography.h3` for list item titles
- `Theme.typography.caption` (14px) for meta text — use 12px
- `Theme.spacing.lg` (24px) padding for list item cards
- Hardcoded color values — use Theme constants

---

## Step Indicators (Wizards)

- 10x10px dots, borderRadius 5
- Active: solid `Theme.colors.primary`
- Completed: primary with 0.5 opacity
- **NO labels, NO checkmarks, NO numbers**

## Buttons

| Type | Style | Use Case |
|------|-------|----------|
| Primary | `Theme.buttons.solid` | Main actions |
| Publish | Green/success | Final submit |
| Outline | Border only | Secondary actions |

- Full-width in wizards
- Never split with other actions

## Audio Waveform Colors

- Not playing: ALL bars = `Theme.colors.primary`
- Playing: played = primary, unplayed = border

## Text Inputs

- Indigenous language: `autoCapitalize="none"`
- Light background with border for previews

## Stack Screen Headers

- Header: `Theme.colors.primary` background, white text
- Body: `Theme.colors.background`

## Colors

| Name | Value | Use |
|------|-------|-----|
| Primary | `#305880` | Headers, buttons, accents |
| Secondary | `#4682B4` | Alternate blue |
| Success | `#27AE60` | Publish buttons |
| Text primary | `#2C3E50` | Main text |
| Text secondary | `#7F8C8D` | Meta, labels |
| Background | `#FAFAFA` | Page backgrounds |
| Surface | `#FFFFFF` | Cards |
| Border | `#E1E8ED` | Dividers |
