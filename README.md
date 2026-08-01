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
  `.field-label`, rows are a fixed 40px — override with `--table-row-h`, NOT
  `--row-h`, which the ledger defines globally for its own cells; opt rows in
  with `data-clickable` / `data-selected`), and `.row-accent` (Airtable-style left
  stripe on a table row flagging a row-level state; colour via `data-accent`,
  in two families — the Butiá-domain keys `human|novo|replied|cold` (the state
  of a PERSON in a conversation) and the generic `danger|warn|ok|muted`, for a
  table whose state isn't a person's (a bounced send, an overdue bill) →
  `--signal-red`/`--signal-yellow`/`--signal-green`/`--signal-gray`,
  the vivid signal colours — brighter than the earth-tone status tokens, since
  a stripe must pop), and `.form-sheet` — the standard form window (see
  **Patterns** below).
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
  - `.form-label` — the name of a field you FILL IN, as opposed to
    `.field-label`, which names a value you READ (Inter 13 / 500 /
    sentence case / foreground). At 10.5 uppercase muted an editable
    field's label ends up weaker than its own placeholder.

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

- **Form window.** `.form-sheet` is the shape of every form dialog in the
  panel: `-head` (title + one supporting line), `-body` (the fields),
  `-foot` (the actions, which stay put while the body scrolls). Pair the
  title with `.card-title` and each field's label with `.form-label`.

  **No rules anywhere.** The window is the page grey (`--background`) and
  the controls are white (`--card`) with no border — one hairline shadow
  each. The change of colour between field and ground is the only
  separator: no band under the title, no line above the foot, no box
  around a group. Focus is carried by the ring alone, which is why the
  control keeps a transparent 1px border (nothing shifts when it paints).

  The body is always a 12-column grid — the bed and its rulers are
  `.form-grid`, which `.form-sheet-body` implies. Put `.form-grid` on any
  block of fields that needs the same geometry without the sheet's window
  chrome (the proposal record's tabs do exactly that). A `.form-field`
  declares `data-span` from the TYPE OF DATA it holds — 2 for a short number, 3 for
  a date, 4 for a select or a phone, 6–8 for a name, 12 for free text —
  and the window picks the ruler with `data-layout`:

  - `coluna` (default) — **creating**. Filled in sequence, so a short field
    takes half a line and everything else takes the line. 560px.
  - `grade` — **editing** a record that already exists. Spans are honoured
    as written, so a dozen fields fit without scrolling. 900px.

  Same markup either way. Help text sits above the control (`.form-hint`),
  and is hidden automatically on a field narrower than a third of the
  window — there the explanation belongs in the `.info-hint` tooltip.

  The ruler answers to the GRID's width, not the window's (container
  query): under ~620px the spans double up, under ~480px everything takes
  the line. A viewport media query would miss the case that matters — a
  wide window whose fields sit in a narrow column beside a summary.

  A control is always the opposite tone of what it sits on: white on the
  sheet's grey by default, grey on a white card when the grid says
  `data-surface="card"`. With no border to carry the edge, two matching
  tones would erase the field.

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
