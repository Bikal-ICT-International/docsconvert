# DOCX to Markdown (Pandoc in GitHub Actions + Cloudflare Worker)

This repo converts Word `.docx` files to GitHub-flavored Markdown and extracts embedded media **without** requiring users to install Pandoc locally. The public web UI is hosted on GitHub Pages, while a Cloudflare Worker securely triggers GitHub Actions and streams the output artifact back to the browser.

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
const WORKER_BASE = "https://docsconvert.shrestha.cv";
```


