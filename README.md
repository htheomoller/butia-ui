# @butia/ui

Butiá design system — the shared tokens and component styles behind
**butia-dashboard** and **ledger**. Change a value here, tag a release,
bump the tag in both apps: the design updates everywhere.

The public site (obutia.com) does **not** use this package.

## What's inside

- `tokens.css` — the four palette blocks:
  - `:root` / `.dark` — warm earth tones (Butiá brand, customer-facing
    surfaces like /proposta and /portal)
  - `:root.admin` / `:root.admin.dark` — neutral-grey admin palette
    (hue 220). Scoped to `html.admin`: the dashboard's Shell adds the
    class on admin routes; the ledger sets it permanently.
  - Plus semantic tokens (`--success/--warning/--destructive`,
    `--income/--expense/--transfer/--warn/--danger`), charts, sidebar,
    shadows, `--radius`.
- `components.css` — class-based pieces of the admin look:
  `.seg`, `.seg-timeline` segmented controls (the selected item reads
  heavier — weight 600 — so the active choice is unmistakable), `.kbd`,
  `.card-cta` (clickable "card com chamada": 80%-width copy column +
  underlined CTA line; the whole card is the click target), `.split-pane`
  (one white frame split into columns by a hairline — list/table left,
  editor/detail right; set the split with a grid utility on the element),
  `.info-hint` (the superscript "ℹ" trigger next to a title — muted icon
  that lifts on hover/focus; the app's own tooltip renders the white help
  bubble, so collision handling lives with Radix/Base UI, not here),
  `.back-link` (the quiet "‹ Parent" link above a `.page-title` on any
  child page), `.filter-bar` (a row of filter controls forced to one
  shared height — `--control-h`; set it to `var(--row-h)` to align with a
  data table), `.checkbox` (the small square toggle used in
  multi-select menus and step lists), `.pill` (the neutral grey
  informational badge — channel, tags — one flat chip family; status colour
  lives on `.row-accent`, not here) with its `.pill-caps` modifier (same
  chip, small/bold/uppercase — the one-word qualifier beside a heading,
  e.g. "HOJE" next to a group band's date), `.table-simple` (a data table with
  the DataGrid's cell language but none of its machinery — for split-pane
  master lists and short standalone tables; box only, pair each `th` with
  `.field-label`, override `--row-h`, opt rows in with
  `data-clickable` / `data-selected`), and `.row-accent` (Airtable-style left
  stripe on a table row flagging a row-level state; colour via
  `data-accent="human|replied|cold"` → `--signal-amber`/`--signal-green`/`--signal-red`,
  the vivid signal colours — brighter than the earth-tone status tokens, since
  a stripe must pop).
- `typography.css` — the shared type scale (class-based, framework
  neutral). Being defined incrementally:
  - `.page-title` — H1 at the top of every admin page (Inter 48 / 600 / -0.03em).
  - `.card-title` — heading inside a card or section (Inter 24 / 600 / -0.01em).
  - `.page-subtitle` — muted 14px supporting / caption text (the quiet
    secondary voice; bakes in --muted-foreground).
  - `.field-label` — the small uppercase label that names a value: a data
    table's column header, the caption above a field in a panel or editor,
    the key in a key/value row (Inter 10.5 / 500 / 0.07em / muted). The most
    repeated piece of type in the admin tool.
  - `.section-title` — the heading of a card or block, one step below the
    card title (Inter 15 / 600 / -0.01em / foreground). Same size band as
    `.page-subtitle`; weight and colour carry the difference (heading vs
    supporting voice), not size.
  - `.section-label` — the small-caps band naming a REGION of a page
    ("Fila e enviados"). Same family as `.field-label`, one step up
    (12.5 / 600 / 0.06em / muted). Use `.section-title` for sentence case.
  - `.field-label-quiet` — the same label one step back, for use inside a
    popover or menu where a full-strength label would compete with the
    content it introduces (10px, half-faded).

## Patterns

Two conventions that are part CSS class, part app composition (like
`.info-hint`, the design system ships the framework-neutral half):

- **Child-page header.** Any page reached from a parent ("mother") page
  opens with a `.back-link` ("‹ Parent") above the `.page-title`, then the
  title row with an optional action on the right. The back link is the
  standard way back — a child page never leaves you stranded.
- **Filter bar with multi-select.** A `.filter-bar` holds the page's
  filter controls at one shared height (`--control-h`, aligned to the
  table with `var(--row-h)`): segmented `.seg` "pills", a search input,
  and a **multi-select dropdown** for choosing several items at once
  (e.g. accounts). The dropdown pattern: a trigger whose label reflects
  the selection ("Todas" / "N contas" / a single name), a panel with
  **"select all" / "clear all"** actions and a `.checkbox` list. The
  popover shell itself comes from each app's primitive (Base UI / Radix);
  the design system supplies the `.filter-bar`, `.seg`, and `.checkbox`
  pieces.

  > Adoption across existing pages is incremental — page by page — not a
  > single sweep.

## Usage

```json
"dependencies": {
  "@butia/ui": "github:htheomoller/butia-ui#v0.1.0"
}
```

```css
/* at the top of your app's global stylesheet */
@import "@butia/ui/tokens.css";
@import "@butia/ui/components.css";
@import "@butia/ui/typography.css";
```

Each app keeps its own `@theme inline` block mapping the CSS variables
into Tailwind color utilities — that's framework wiring, not design.

## Releasing a change

1. Edit the CSS.
2. Bump `version` in package.json, commit.
3. `git tag vX.Y.Z && git push --tags`
4. In each app: update the tag in `package.json`, `npm install`,
   verify, deploy.

Pinned tags are deliberate: a design change can never ship to an app
without an explicit version bump in that app's repo.
