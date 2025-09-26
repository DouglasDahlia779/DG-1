

**How updates work (your flow)**
- Edit/add Markdown in Obsidian → symlink writes into DG-1/quartz/content
- Commit & push from DG-1 → GitHub Actions rebuilds & deploys automatically
- [[Publishing DG-1 -- how to]]

**Change the site title (“Quartz 4” → your title)**
1) Open the config
open -e quartz/quartz.config.ts
2) In the exported config, find the `configuration` block and set:
  pageTitle: "Your Title",
  pageTitleSuffix: "",
  baseUrl: "douglasdahlia779.github.io/DG-1"   # good for RSS/sitemaps on GH Pages
(Fields documented in Quartz docs: `pageTitle`, `pageTitleSuffix`, `baseUrl`.)  ← see sources

**Add a site icon (favicon in the browser tab)**
1) Prepare a square PNG (transparent if you like), ~512×512, name it `icon.png`
2) Put it here (create folder if needed)
mkdir -p quartz/static && cp /path/to/your/icon.png quartz/static/icon.png
(Quartz’s Favicon emitter turns `quartz/static/icon.png` into `favicon.ico` automatically.)  ← see sources

**Commit & deploy**
git add quartz/quartz.config.ts quartz/static/icon.png
git commit -m "Set site title and favicon"
git push

**Verify**
- Watch **Actions** run to green
- Refresh your site: https://douglasdahlia779.github.io/DG-1/

**Optional — want a logo in the header (not just favicon)?**
- Quartz 4 lets you customize layout in `quartz.layout.ts` and add components to the `header` slot. We can add a tiny logo component there next if you want.  ← see sources

**Sources**
- Quartz configuration fields (`pageTitle`, `pageTitleSuffix`, `baseUrl`) are set in `quartz.config.ts`. :contentReference[oaicite:0]{index=0}
- Favicon plugin uses `quartz/static/icon.png` to generate the favicon automatically. :contentReference[oaicite:1]{index=1}
- Layout is customizable via `quartz.layout.ts` header/beforeBody slots. :contentReference[oaicite:2]{index=2}
