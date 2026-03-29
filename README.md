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
const WORKER_BASE = "https://api.docsc.shrestha.cv";
```

## Cloudflare Worker Setup
Create a Worker:
1. Cloudflare Dashboard -> Workers & Pages -> Create application -> Worker
2. Name it (`docsconverter`) and Deploy

Upload the Worker code:
1. Open the Worker -> Edit code
2. Replace contents with `worker/index.js` from this repo
3. Deploy

Add environment variables (Settings -> Variables):
- `GITHUB_OWNER` = your GitHub username or org
- `GITHUB_REPO` = this repo name
- `GITHUB_BRANCH` = `main`

Add a secret (Settings -> Secrets):
- `GITHUB_TOKEN` = fine-grained PAT with:
  - Actions: Read and write
  - Contents: Read (optional)

Optional custom subdomain route:
- Workers & Pages -> your Worker -> Settings -> Triggers -> Routes
- Route: `api.docsc.shrestha.cv/*` -> your Worker

## Usage (Public Site)
1. Open the GitHub Pages URL.
2. Upload your `.docx`.
3. Click Download to get the Markdown + media zip.
4. Edit in the live editor and preview instantly.

## Notes
- The workflow runs only on manual dispatch.
- Outputs are stored as artifacts, not committed to the repo.
