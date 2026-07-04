# onward-reports

Client-facing report site for **Onward**, hosted on GitHub Pages and prepared by Taboo Grow.

**Live site:** https://taboogrow.github.io/onward-reports/

---

## What this is

A static website that hosts client-facing reports for Onward. It exists so we can share a
clean, no-login link with the client instead of a claude.ai artifact (which, on our Team plan,
can only be shared inside the Taboo Grow org).

### Why one repo per client (not one shared repo)

The Taboo Grow GitHub org is on the **free plan**, where GitHub Pages only serves from
**public** repos. A single shared `client-reports` repo would let any client browse the repo
tree and see that *other* clients exist. To keep clients isolated, **each client gets its own
public repo** (`onward-reports`, and the same pattern for future clients). A client can only
ever see their own repo.

> Because the repo is public, treat everything committed here as client-visible. Only commit the
> **client-facing** cut of a report — never the internal version (account names, lost-account
> detail, funnel internals, etc.).

---

## Structure

```
onward-reports/
├── index.html                          Landing page — lists every Onward report
├── README.md
└── reddit/                             One folder per service line
    └── setup-launch-2026-07/           One folder per report (slug = topic + period)
        └── index.html                  The report itself (self-contained HTML)
```

Each report lives in its own folder as `index.html`, which gives a clean directory URL:

| Report | URL |
|--------|-----|
| Reddit — Setup & Launch (Jun 1 – Jul 3, 2026) | https://taboogrow.github.io/onward-reports/reddit/setup-launch-2026-07/ |

Report HTML is fully self-contained: all images are inlined as `data:` URIs and there are no
external CSS/JS dependencies, so each report renders standalone forever.

---

## Add a new report

1. Create a folder: `<service>/<topic>-<YYYY-MM>/` (e.g. `reddit/status-2026-08/`).
2. Save the **client-facing** report HTML into it as `index.html`.
3. Add a card for it in the root `index.html` (copy an existing `<a class="card">` block).
4. Commit and push to `main`:
   ```bash
   git add . && git commit -m "Add <report name>" && git push
   ```
5. GitHub Pages rebuilds automatically in ~30–60s. Verify the new URL loads.

## Add a new client

Repeat this repo pattern: create a new public repo `taboogrow/<client>-reports`, enable Pages
(Settings → Pages → Source: `main` / root), and mirror this structure.

---

## How hosting works

- **Source:** `main` branch, root (`/`) folder.
- **Build:** GitHub Pages (static, no build step).
- **Enable/check:** repo → Settings → Pages, or `gh api repos/taboogrow/onward-reports/pages`.
- Custom domain: none. To add one (e.g. `reports.taboogrow.com`), add a `CNAME` file and a DNS
  record — see [GitHub Pages custom domain docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

## Verify it's working

```bash
curl -sI https://taboogrow.github.io/onward-reports/ | head -1                       # → 200
curl -sI https://taboogrow.github.io/onward-reports/reddit/setup-launch-2026-07/ | head -1  # → 200
```
