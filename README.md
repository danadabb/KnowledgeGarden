# Knowledge Garden

A collection of notes on all things software engineering.

- **Edited with** [Obsidian](https://obsidian.md) — open the `vault/` folder as a vault.
- **Published with** [Quartz v4](https://quartz.jzhao.xyz) → GitHub Pages: <https://danadabb.github.io/KnowledgeGarden/>

## Local development

```bash
npm install          # once
npm run serve        # live preview at http://localhost:8080
npm run build        # one-off build into ./public
```

Notes live in `vault/`. The Quartz framework lives in `quartz/` and is configured
by `quartz.config.ts` / `quartz.layout.ts`.

## Publishing

Pushing to `main` triggers `.github/workflows/deploy.yml`, which builds the site
and deploys it to GitHub Pages. Repo **Settings → Pages → Build and deployment →
Source** must be set to **GitHub Actions**.

## Updating Quartz

```bash
git remote add upstream https://github.com/jackyzha0/quartz.git   # once
git fetch upstream
git merge upstream/v4
```
