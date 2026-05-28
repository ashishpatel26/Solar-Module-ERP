# Deploying this repo to GitHub Pages

Quick steps to publish the site and get the URL you want.

1) Commit & push the new files

```bash
git add index.html DEPLOY.md
git commit -m "Add Pages redirect and deploy instructions"
git push origin main
```

2) Enable GitHub Pages

- Go to the repository Settings → Pages.
- Under "Source" select `main` branch and the `/ (root)` folder, then Save.
- Wait a few minutes for the site to become available.

3) URLs you'll get

- If the repo name stays `Solar-Module-ERP`, the site will be at:
  `https://pinakk1987.github.io/Solar-Module-ERP`
- If you rename the repo to `solar-erp`, the site becomes:
  `https://pinakk1987.github.io/solar-erp` (exactly the URL you requested).

4) Optional: rename the repo (via GitHub web UI or `gh`)

Web: Repository Settings → Rename.

CLI (requires `gh` and authenticated):

```bash
gh repo rename pinakk1987/Solar-Module-ERP --new-name solar-erp
```

5) Alternative: Use a user/organization Pages repo

If you create a repository named `pinakk1987.github.io` and publish its `main` branch, you can place this project in a subpath `/solar-erp` to get `https://pinakk1987.github.io/solar-erp` without renaming.

Notes

- Allow several minutes for DNS/Pages propagation.
- If you want, I can run the `git` commands here and push — tell me to proceed.
