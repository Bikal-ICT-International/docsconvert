# DOCX to Markdown (Pandoc in GitHub Actions)

This repo converts Word `.docx` files to GitHub-flavored Markdown and extracts embedded media without requiring users to install Pandoc locally.

## How it works
- Put `.docx` files in the `input/` folder.
- Push to `main` or run the workflow manually.
- The workflow generates:
  - `output/<filename>/<filename>.md`
  - `output/<filename>/media/*` (extracted images and media)
- Outputs are both:
  - uploaded as a workflow artifact (downloadable zip), and
  - committed back into the repo under `output/`.

## Usage
1. Add your `.docx` files to `input/`.
2. Push to `main` (or use Actions -> "Run workflow").
3. Download the artifact from the Actions run, or check the committed files in `output/`.

## Notes
- The workflow only runs on pushes to `main` and manual runs.
- If there are no `.docx` files in `input/`, nothing is generated.
