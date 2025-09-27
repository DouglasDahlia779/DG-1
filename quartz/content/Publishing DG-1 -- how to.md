
![[DG-1 Update Process.png]]




| Step | What to do                                                        |
| ---- | ----------------------------------------------------------------- |
| 1.   | open iterm                                                        |
| 2.   | open new window in DG- Profile                                    |
| 3.   | pwd:                                                              |
| 4.   | /Users/MAC_at_home/Documents/GitHub/DG-1                          |
|      |                                                                   |
|      | cd quartz (before npx quartz build)                               |
|      | /Users/MAC_at_home/Documents/GitHub/DG-1/quartz                   |
|      | npx quartz build --serve (to test config locally)                 |
|      |                                                                   |
| 5.   | edit notes in Obsidian                                            |
| 5.   | add the tag #dg to move the note to the _quartz dir_              |
|      |                                                                   |
| 6.   | git commit -m "reason for change"                                 |
| 7.   | git push                                                          |
|      | causes site to be re-built                                        |
| ❤️   | site hosted at: [index](https://douglasdahlia779.github.io/DG-1/) |
|      |                                                                   |
|      | CNT+C to kill server window ❤️                                    |
|      | to get an image on the website:                                   |
|      |                                                                   |
|      | GET IMAGES ON HYOUR WEBSITE                                       |
|      | -1- move image from bins into /quartz                             |
|      | -2- CNT+C to kill the localhost                                   |
|      | -3- npx quartz build                                              |
|      | -4- in obsidian, drag image into the note from the /quartz dir    |
|      | -5- npx quartz build --serve                                      |
|      |                                                                   |
|      |                                                                   |
|      |                                                                   |


|     | Configuration Changes | file name |
| --- | --------------------- | --------- |
|     |                       |           |




**How your notes go from Obsidian → HTML → GitHub Pages**

**Big picture pipeline**
- You write/edit/delete notes in Obsidian inside the symlinked folder (which points to `quartz/content`)
- Quartz reads everything in `content/`, converts Markdown → HTML, and puts the site in `public/`
- You commit/push (or run `npx quartz sync`) to GitHub
- GitHub Actions builds & publishes the `public/` folder to GitHub Pages

**What happens at each stage**

| Stage | You do | Quartz/GitHub does | Output |
|---|---|---|---|
| Author | Create/edit/delete notes in the symlink folder inside your **Studies** vault | — | Markdown files in `quartz/content` |
| Build (local preview) | `cd quartz && npx quartz build --serve` | Parses Markdown (incl. Obsidian features), generates static site | Built site in `quartz/public`, live at http://localhost:8080 |
| Sync to GitHub | `npx quartz sync` (or manual `git add/commit/push`) | Commits your content & config, pushes to your repo | GitHub has your latest notes/config |
| Deploy (GitHub Pages) | Nothing—push triggers CI | GitHub Action runs `npx quartz build`, uploads `public` as the Pages artifact | Your site updates at `<username>.github.io/<repo>` |

**Details that matter**

- **Content folder**  
Quartz looks in `content/` by default (`-d` flag can change it). Because you symlinked a folder inside your Studies vault to `quartz/content`, anything you write there is “seen” by Quartz immediately. :contentReference[oaicite:0]{index=0}

- **Output folder**  
The static site (HTML/CSS/JS) is emitted to `public/` during a build (`-o` can change it). The `--serve` flag starts a hot-reloading preview at `http://localhost:8080`. :contentReference[oaicite:1]{index=1}

- **Markdown → HTML conversion**  
Quartz supports standard Markdown plus Obsidian-friendly syntax: wikilinks `[[...]]`, callouts, tables, task lists, footnotes, etc. You can add frontmatter to control titles, dates, tags, etc. :contentReference[oaicite:2]{index=2}

- **Pushing changes**  
`npx quartz sync` is a helper that (1) optionally pulls, (2) commits, and (3) pushes your changes to your GitHub repo. You can also do plain Git yourself. :contentReference[oaicite:3]{index=3}

- **GitHub Pages deployment**  
With the recommended workflow file in `.github/workflows/deploy.yml`, a push to your chosen branch (often `v4`) triggers GitHub Actions to run `npx quartz build` and publish the `public/` directory to Pages. Note: Quartz outputs `file.html` (no trailing slash); GitHub Pages doesn’t auto-redirect trailing slashes. :contentReference[oaicite:4]{index=4}

**Create/modify/delete behavior**

- **Create** a note in the symlink → it appears under `quartz/content/...`. On build, Quartz generates `public/<path>/<file>.html` and it goes live after the CI deploy. :contentReference[oaicite:5]{index=5}
- **Modify** a note → rebuild regenerates the corresponding HTML; push/deploy updates the live page. :contentReference[oaicite:6]{index=6}
- **Delete** a note → the Markdown disappears from `content/`. After a new build, the HTML will no longer be generated. When you push, the deleted file is removed from the published site in the next deploy (links to it will 404 unless you update or add redirects). :contentReference[oaicite:7]{index=7}

**Exact commands you’ll use (macOS terminal)**
- Preview locally while you write:

- Push changes to GitHub (and trigger Pages deploy):

cd /Users/MAC_at_home/Documents/GitHub/DG-1/quartz  
npx quartz sync


**Recommended GitHub Pages workflow (once)**
1) Create `.github/workflows/deploy.yml` in your Quartz repo with the Quartz docs’ example (branch, Node 22, `npx quartz build`, publish `public/`)  
2) In your repo **Settings → Pages**, set **Source = GitHub Actions**. After your next `npx quartz sync`, the Action builds and publishes automatically. :contentReference[oaicite:8]{index=8}

**Sources (docs & guides)**
- Build/serve flags, defaults (`content` → `public`): https://quartz.jzhao.xyz/build :contentReference[oaicite:9]{index=9}
- Authoring (Obsidian features & frontmatter): https://quartz.jzhao.xyz/authoring-content :contentReference[oaicite:10]{index=10}
- Sync command & options: https://quartz.jzhao.xyz/setting-up-your-GitHub-repository :contentReference[oaicite:11]{index=11}
- GitHub Pages CI workflow & caveats: https://quartz.jzhao.xyz/hosting :contentReference[oaicite:12]{index=12}

**TL;DR**
- Write in Obsidian (inside the symlinked folder) → `content/`
- Preview: `npx quartz build --serve` → `public/` locally
- Publish: `npx quartz sync` → GitHub Actions builds & updates GitHub Pages

![[Divider_3.svg]]


Q what terminal commands must I enter to (1) convert my markdown into html (2) to send a copy to Github

**You only need two steps:**

**1) Convert Markdown → HTML (build the site locally)**
```bash
cd /Users/MAC_at_home/Documents/GitHub/DG-1/quartz
npx quartz build --serve
npx quartz build = converts all Markdown in content/ → HTML in public/

--serve = also starts a preview server at http://localhost:8080

If you don’t want the preview, just run:

bash
Copy code
npx quartz build
2) Send (push) your site to GitHub
Option A (simplest, Quartz helper command):

bash
Copy code
npx quartz sync


This will:

- Stage (`git add .`)
    
- Commit (`git commit -m "update"`)
    
- Push to your GitHub repo (you must have set up the remote first).
    

Option B (do Git manually):

`git add . git commit -m "update notes" git push origin main`

(or `master`/`v4` depending on your branch name)

**Summary**

- Build (Markdown → HTML): `npx quartz build`
    
- Publish to GitHub: `npx quartz sync` (or use the three Git commands)
    

👉 Do you want me to also show you the **very first time setup commands** (to connect your repo on GitHub to this Quartz folder), or do you already have `origin` set up?

```



![[Divider_3.svg]]

**Where your Quartz site is hosted on GitHub**

Your Quartz notes get published through **GitHub Pages**.  
The exact URL depends on **(a)** whether you use a *user site* or a *project site*.

**1. User site (special case)**
- Repo must be named `<username>.github.io`
- Example: if your GitHub username is `DouglsDahlia779` and the repo is `DouglsDahlia779.github.io`
- Pages URL → `https://DouglsDahlia779.github.io/`
- This is the “root” site for your account

**2. Project site (normal case for Quartz)**
- Repo can have any name (yours is `DG-1`)
- Pages URL → `https://<username>.github.io/<repo-name>/`
- For you: `https://DouglsDahlia779.github.io/DG-1/`

**3. Custom domain (optional)**
- If you buy your own domain (e.g., `mygardennotes.ca`)
- You can configure it in GitHub → Settings → Pages → “Custom domain”
- Then your site shows at that domain instead

**How it gets online**
- Each `git push` runs GitHub Actions (the workflow file in `.github/workflows/deploy.yml`)
- The Action builds your site (`npx quartz build`) and publishes the `public/` folder to GitHub Pages
- GitHub Pages serves the result at one of the URLs above

---

👉 Do you want me to walk you through setting your repo so it uses the **project site URL** (`.../DG-1/`) or are you thinking about renaming your repo to be a **user site** (so it’s just `...github.io/`)? 



