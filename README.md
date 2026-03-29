# DOCX to Markdown (Pandoc in GitHub Actions + Cloudflare Worker)

This repo converts Word `.docx` files to GitHub-flavored Markdown and extracts embedded media. The public web UI is hosted on GitHub Pages, while a Cloudflare Worker securely triggers GitHub Actions and streams the output artifact back to the browser.

## How it works
- The public web UI uploads your `.docx` to a temporary file host (file.io).
- A Cloudflare Worker triggers the GitHub Actions workflow with that temporary URL.
- The workflow converts the file and uploads the results as an artifact.
- The Worker streams the artifact back to the browser.
- Nothing is committed to the repo.

## GitHub Pages Setup (Public UI)
Enable GitHub Pages (repo Settings -> Pages) and select:
- Source: `Deploy from a branch`
- Branch: `main` / `/docs`

Update the Worker URL in `docs/app.js`:
```
const WORKER_BASE = "https://api.docsconvert.shrestha.cv";
```

## Cloudflare Worker Setup
Create a Worker:
1. Cloudflare Dashboard -> Workers & Pages -> Create application -> Worker
2. Name it (`docsconverter`) and Deploy

Upload the Worker code:
1. Open the Worker -> Edit code
2. Replace contents with `worker/index.js` from this repo
3. Deploy
