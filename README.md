# Portfolio (static site)

## Deploy on Vercel

1. Push this folder to a GitHub repository (see below).
2. In [Vercel](https://vercel.com): **Add New Project** → Import the GitHub repo.
3. **Framework Preset:** Other (or “No framework”). Root directory: `.` (repository root).
4. Deploy. Vercel serves the root URL `/` from `index.html` in the project root (no extra config needed).

No build step is required.

## Images missing after deploy

**If every image under `assets/` is broken online but CDN URLs work:** Vercel only deploys what is in your Git repository. If the `assets/` folder (or subfolders like `assets/SocialEase/`) exists on your computer but was never committed and pushed, those files do not exist on the server and every `src="assets/..."` request returns 404.

Fix: run `git add assets` (or `git add .`), `git commit`, `git push`, then redeploy. Confirm on github.com that you can see the same folders and files under **assets** in the repo.

**If you uploaded images one-by-one on the GitHub website** (so pictures exist at the repo root or in random paths, but there is no `assets/` folder): the site will still request `assets/...` and get 404. You need the **same folder layout as this project on your computer**: an `assets` folder containing the subfolders and files (e.g. `assets/map-of-isu.png`, `assets/SocialEase/slides-png/...`). Easiest fix: clone the repo locally, copy your local `assets` folder into the clone, then `git add assets`, commit, and push—or use GitHub Desktop to add the entire `assets` directory in one go.

Production (Vercel runs Linux) is **case-sensitive**: `assets/photo.png` and `Assets/photo.png` are different paths. Match folder and file names exactly to what is in the repo.

Filenames with spaces or special characters may need URL-encoding in `src` attributes, or rename files to simple ASCII names.

## Assets folder too large for GitHub

GitHub blocks single files **over 100 MB**. Between **50–100 MB** you get a warning; large repos also push slowly or time out in the browser.

**Important:** The site loads images from paths like `assets/...`, not from `assets.zip`. Do not upload only a zip expecting images to work—you need the **unzipped** `assets` folder in the repo (or another hosting approach below).

**Practical options (pick one or combine):**

1. **Shrink the folder first (often enough)**  
   - Compress PNG/JPEG for web (e.g. [TinyPNG](https://tinypng.com/), [Squoosh](https://squoosh.app/)).  
   - Export slide decks as **smaller dimensions** if they are only shown at web size.  
   - **Do not commit huge PDFs** if the HTML only uses PNG slides—keep PDFs out of the repo or store them elsewhere.

2. **Git Large File Storage (LFS)**  
   Install [Git LFS](https://git-lfs.com/), then in your repo run e.g. `git lfs track "*.png"` and `git lfs track "*.pdf"`, add `.gitattributes`, commit, then add your `assets` and push. Large binaries are stored as LFS pointers; GitHub includes a limited amount of LFS storage/bandwidth on free accounts—check current [Git LFS pricing](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage).

3. **Host heavy images outside the repo**  
   Upload images to a CDN or object storage (e.g. Cloudinary, Cloudflare R2, S3 + public URL), then change `src` in HTML to the **full HTTPS URL**. Your project already uses remote URLs for some pages; the same idea for `assets`-heavy pages.

4. **Deploy from your computer with Vercel CLI** (advanced)  
   `vercel` can upload your local project in one shot, which can avoid pushing multi‑GB folders through Git—but the usual workflow is still “code on GitHub”; use this only if you understand that **Git and what’s live can diverge** unless you automate sync.

## Push to GitHub (first time)

From this directory in Terminal:

```bash
cd /path/to/portfolio
git init
git add .
git commit -m "Initial portfolio site"
```

Create an empty repo on GitHub, then:

```bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git branch -M main
git push -u origin main
```

Replace `YOUR_USERNAME` and `YOUR_REPO` with your GitHub account and repository name.

## Local preview

Open `index.html` in a browser, or run a static server from this folder:

```bash
npx --yes serve .
```

Then open the URL shown (e.g. `http://localhost:3000`).
