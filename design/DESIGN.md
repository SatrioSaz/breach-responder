# Breach Responder — Design System

Design tokens and component specs for the Breach Responder UI. These match the
interactive walkthrough in `visuals/breach-responder-walkthrough.html`. The SVG
artboards in this folder are built from these tokens and import directly into
Figma (File → Place image / drag onto canvas; SVGs come in as editable layers).

## Artboards

Each screen is a **1280 × 720** frame (16:9, matches the demo video).

| File | Screen | Purpose |
|---|---|---|
| `screen-1-ask.svg`      | Ask      | Trigger + input form (company, aliases, folders) |
| `screen-2-classify.svg` | Classify | PII exposure list — the "type and location, never the value" moment |

## Color tokens

| Token | Hex | Use |
|---|---|---|
| `ground`     | `#0F1720` | App background |
| `panel`      | `#16212E` | Main content panel |
| `panel-2`    | `#1E2B3A` | Cards, raised surfaces |
| `line`       | `#263445` | Hairline borders |
| `line-2`     | `#3A4B5E` | Stronger borders, inputs |
| `text`       | `#E6EDF3` | Primary text |
| `text-2`     | `#C7D3DF` | Secondary text |
| `muted`      | `#9FB0C2` | Captions, labels |
| `green`      | `#34D399` | Primary action, success, brand |
| `green-bg`   | `#123A2E` | Green chip/pill background |
| `green-text` | `#A7F3D0` | Green text on dark |
| `green-ink`  | `#06291D` | Text on green button |
| `amber`      | `#F5B65A` | Warnings, approval, needs-review |
| `amber-bg`   | `#3A2C12` | Amber chip background |
| `amber-text` | `#FCD9A0` | Amber text on dark |
| `amber-ink`  | `#2A1C04` | Text on amber button |
| body backdrop | `#0B121A` | Behind the 1280×720 stage |

## Typography

- **Inter** — UI text. Weights: 400, 500, 600, 700.
- **JetBrains Mono** — file paths, technical values. Weights: 400, 500.

| Role | Size / weight |
|---|---|
| H (screen title) | 22px / 700 |
| Brand name | 20px / 700 |
| Body | 15px / 400 |
| Label | 13px / 500 |
| Eyebrow | 13px / 600, uppercase, letter-spacing 0.08em |
| Caption / muted | 13–14px / 400 |
| Mono path | 13px / 400 |

## Spacing & radius

- Stage padding: 28px top/bottom, 40px left/right
- Panel padding: 24px
- Card padding: 18–20px
- Gaps: 8 / 10 / 16 / 24px
- Radius: pill 999px · button/input/chip 10px · card 14px · panel 16px
- Buttons: 44px tall (primary CTA 48px)

## Key components

- **Brand lockup** — 40×40 rounded square (`green-bg`) with a shield-check icon, name + tagline.
- **Local-only pill** — rounded pill, lock icon, `green-bg` / `green-text`.
- **Step nav** — 5 equal cells (Ask · Find · Classify · Review · Save). Active = solid green; done = green outline + check; upcoming = muted.
- **Exposure row** — 3 columns: data-type (600 weight) · source file (mono, `text-2`) · confidence badge (High = green, Medium = amber).
- **Privacy notice** — eye-off icon + "Values are never shown or saved — only the type of data and the file it is in."
- **Buttons** — primary (green, `green-ink` text), ghost (transparent, `line-2` border), amber (approval/save).
