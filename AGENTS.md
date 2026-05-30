# Agent Instructions for l33tLumberjack.github.io

This repository is a GitHub Pages site built using **MkDocs** with the **Material for MkDocs** theme.

## Project Overview
- **Purpose**: A personal blog and documentation site.
- **Source Content**: All primary content (Markdown files) resides in `build/docs/`.
- **Build System**: MkDocs.
- **Configuration**: The build configuration is located at `build/mkdocs.yml`.

## Workflow Guidelines

### 1. Modifying Content
- **Do not edit files in the root or `Blog/` directories directly.** These appear to be generated HTML outputs.
- **Always edit Markdown files in `build/docs/`.**
    - Blog posts are located in `build/docs/Blog/`.
    - The homepage is `build/docs/index.md`.
- Drafts in `interview-template/new_post/` are staging files only. Promote finished posts by copying them into `build/docs/Blog/`, then remove the draft copy if it is no longer needed.
- After modifying a `.md` file, you must trigger a rebuild to update the site.

### 2. Building the Site
To apply changes to the web view (`build/site` or root HTML files), run the following from the repository root:
```bash
cd build && mkdocs build
```
*Note: Ensure you have `mkdocs` and `mkdocs-material` installed in your environment.*

If `mkdocs` is not available globally, create a temporary virtual environment and use that binary:
```bash
python3 -m venv /tmp/l33t-mkdocs-venv
/tmp/l33t-mkdocs-venv/bin/python -m pip install mkdocs-material
cd build && /tmp/l33t-mkdocs-venv/bin/mkdocs build
```

### 3. Assets and Images
- **Images**: New images should be placed in `build/docs/img/` or the appropriate subfolder within it to ensure they are included in the build.
- **Styles/Scripts**: Modifications to CSS or JS should be made in `assets/`, but be aware that these may be minified during the build process.

### 4. Deployment
This site is intended for deployment to GitHub Pages. Ensure that any changes to the structure are reflected in the generated `build/site` directory.

## Important Notes
- If you encounter errors during the build, check the `mkdocs.yml` configuration for path mismatches or missing dependencies.
- Always verify that your Markdown syntax is correct to prevent broken rendering in the Material theme.
