# Remedy DS — Component Specs Library

A static, self-contained component documentation site for Loblaw Digital's Remedy Design System.  
No backend. No build step. Drop files, push to GitHub, it's live.

---

## Structure

```
remedy-specs/
├── index.html              ← Component browser
├── spec.html               ← Generic spec template (reads ?id= from URL)
├── data/
│   ├── components.json     ← All component data (the "database")
│   └── tokens.json         ← Design tokens (from Tokens Studio export)
└── .github/workflows/
    └── deploy.yml          ← Auto-deploy to GitHub Pages on push
```

---

## Deployment (one-time setup)

1. **Create a GitHub repo** (private or public — Pages works on both with a paid plan)
2. Push this folder as the repo root
3. Go to **Settings → Pages → Source → GitHub Actions**
4. Push anything to `main` — it deploys automatically

That's it. Your URL will be:  
`https://<org>.github.io/<repo-name>/`

### Custom domain (optional)
Add a `CNAME` file to the root with your domain, e.g.:
```
remedy.loblaw.ca
```
Then point the DNS CNAME record to `<org>.github.io`.

---

## Adding a new component

1. Get the **Dev Mode HTML** from Figma (Dev Mode panel → Copy → CSS/HTML)
2. Get the **Figma node ID** from the URL: `?node-id=XXXX-YYYY`
3. Give both to Claude with: component name, variant, category
4. Claude writes the JSON entry → paste into `data/components.json`
5. `git add . && git commit -m "feat: add [ComponentName]" && git push`
6. Live in ~30 seconds

---

## Updating design tokens

When tokens change in Figma:

1. In Tokens Studio: **Export → JSON → Single file** → save as `data/tokens.json`
2. `git add data/tokens.json && git commit -m "chore: update tokens" && git push`

---

## Component JSON schema

Each entry in `data/components.json → components[]`:

```jsonc
{
  "id": "unique-kebab-id",           // used in URL: spec.html?id=this
  "name": "Component Name",
  "variant": "Variant Name",         // optional
  "category": "Overlay",            // for grouping in the browser
  "description": "...",
  "status": "published",            // "published" | "draft"
  "figmaFileId": "abc123",
  "figmaNodeId": "1234-5678",
  "dimensions": { "width": 358 },

  "devModeHtml": "...",             // raw Dev Mode HTML, {{ID}} as prefix placeholder
  "anatomy": [...],                 // callout points
  "spacingZones": [...],            // inspector hover zones
  "spacingTable": [...],            // right-panel table
  "colors": [...],                  // colour swatches
  "typography": [...],              // type samples
  "cssOutput": "..."                // copyable CSS
}
```

Full schema reference: see the Dialog entry in `components.json`.

---

## Local development

```bash
# Python (built-in)
python3 -m http.server 8765

# Then open:
open http://localhost:8765/index.html
```

No npm, no bundler, no dependencies.
