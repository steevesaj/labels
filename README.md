# Isolation Label Generator

Browser-based generator for sequential electrical isolation-point label sheets — Avery 5163, 10-up, no install or login.

A single self-contained HTML file. Enter a number range or specific tags, and it renders a print-ready PDF **entirely in the browser** — no server, no backend, no software to install. Built so field techs can self-serve from a SharePoint link.

---

## For techs — how to use it

1. Open the tool (link on the SharePoint **Tools** page).
2. Choose **Number range** (e.g. `5000` to `5200`) or **Specific tags** for reprints (e.g. `5042, 5108, 5511`).
3. Check the live preview and the label/sheet count.
4. Click **Generate PDF** — it downloads a positioned sheet.
5. Print on the correct label stock (Avery 5163, 10 per sheet). Don't scale to fit — print at 100% / actual size.

---

## For the admin — hosting

The file is static, so any static host works. Pick one:

| Host | Notes |
|---|---|
| **GitHub Pages** | Free, fastest. Public URL — anyone with the link sees the page and embedded logo. Fine for non-sensitive use. |
| **Azure Static Web Apps** | Free tier; can stay anonymous or be gated to your org via Entra. Use if it must be internal-only. |
| **SharePoint link** | Host on one of the above, then add the URL as a link or Embed web part on the Tools page. Techs are already in M365, so no extra login. |

**Deploy steps (GitHub Pages):**
1. Rename the file to `index.html` and put it in a repo (suggested repo name: `label-generator`).
2. Repo **Settings → Pages** → deploy from your branch. You get a URL like `youruser.github.io/label-generator/`.
3. Add that URL to the SharePoint Tools page.

> Note: don't upload the HTML *into* SharePoint and expect it to run — modern SharePoint blocks scripts in uploaded files. Host it, then link to it.

---

## For the admin — updating

Everything you'd change lives at the top of the `<script>` block in the HTML.

- **Geometry / calibration (most important):** the `MARGIN_*`, `GUTTER`, and `LABEL_*` constants are points (72 pt = 1 inch), set to Avery 5163. If tags print misaligned, nudge these and re-test. **Always test-print on the real stock before rollout.**
- **Logo:** paste the logo's base64 data URL into `LOGO_DATAURL` to bake it into the file permanently. (The in-page "Load logo" picker is for testing only — it doesn't persist.)
- **Fixed text / phone:** phone and prefix are editable in the UI; the header and footer text are in the `drawLabel()` function.
- **New label types later:** the engine is structured so additional companies/label types become config, not a rewrite.

---

## Versioning

Semantic Versioning — `MAJOR.MINOR.PATCH`:

- **PATCH** (`1.0.x`) — geometry tweaks, logo swap, copy fixes.
- **MINOR** (`1.x.0`) — new label type/company, new field or option.
- **MAJOR** (`x.0.0`) — structural redesign or any breaking change.

The version is the single source of truth in one constant (`VERSION`) and surfaces in three places:
1. The **header badge** in the UI (`v1.0.0`).
2. The **PDF metadata** of every generated sheet (creator + keywords include the version, the generation date, and the tag range) — so any printed sheet is traceable to the exact build and parameters that produced it.
3. The **changelog** below.

**To release a new version:** update `VERSION` and `BUILD_DATE` at the top of the file, add a changelog entry, and (if using git) tag the commit `vX.Y.Z`.

### Changelog

#### v1.0.0 — 2026-06-17
- Initial release. Isolation-point tags (proof of concept).
- Range and specific-tag (reprint) modes; editable prefix, padding, and phone.
- Client-side PDF, Avery 5163 (2"×4", 10-up); live preview; version stamped in UI and PDF metadata.

---

## Calibration warning

Print geometry must match the physical label stock exactly. Before any rollout: generate one sheet, print on the actual labels at **100% scale**, and confirm registration. Re-tune the geometry constants if needed, then lock it.
