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
  `.seg`, `.seg-timeline` segmented controls, `.kbd`.

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
