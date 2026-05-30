# l33tLumberjack.github.io

This repository hosts the static website for [l33tLumberjack](https://github.com/l33tLumberjack), built using **MkDocs** with the **Material for MkDocs** theme.

## 🚀 Overview

The site serves as a personal blog and documentation hub, featuring technical write-ups on topics such as cybersecurity, networking (SSL/TLS), and hardware projects (Pwnagotchi).

## 🛠️ Tech Stack

- **Static Site Generator**: [MkDocs](https://www.mkdocs.org/)
- **Theme**: [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- **Search Engine**: [Lunr.js](https://lunrjs.com/) (integrated via MkDocs)
- **Hosting**: GitHub Pages

## 📂 Project Structure

- `build/docs/`: The source of truth. Contains all Markdown (`.md`) files for the site.
- `build/mkdocs.yml`: The primary configuration file for the MkDocs build process.
- `build/site/`: The generated HTML output ready for deployment.
- `assets/`: Static assets including CSS, JavaScript, and images used by the site.
- `Blog/`: Generated HTML directory containing individual blog post pages.

GitHub Pages serves this repository from the root, so root-level generated files such as `index.html`, `404.html`, `Blog/`, `assets/`, `search/`, and sitemap files must stay in sync with `build/site/`.

## ⚙️ Development & Build Instructions

### Prerequisites

Ensure you have Python installed, then install the necessary dependencies:

```bash
pip install mkdocs-material
```

If `mkdocs` is not installed globally, use a virtual environment:

```bash
python3 -m venv /tmp/l33t-mkdocs-venv
/tmp/l33t-mkdocs-venv/bin/python -m pip install mkdocs-material
```

### Local Development

To preview the site locally with live reloading:

```bash
cd build
mkdocs serve
```
Once running, navigate to `http://127.0.0.1:8000` in your browser.

### Building for Deployment

To generate the production-ready HTML files in the `build/site` directory:

```bash
cd build
mkdocs build
```

Then sync the generated deployment output back to the repository root:

```bash
cd ..
rsync -av --delete build/site/Blog/ Blog/
rsync -av --delete build/site/assets/ assets/
rsync -av --delete build/site/search/ search/
cp build/site/index.html build/site/404.html build/site/sitemap.xml build/site/sitemap.xml.gz .
```

Do not sync only `Blog/`. MkDocs Material uses hashed CSS and JavaScript filenames, so the deployed HTML and `assets/` directory must be updated together or the live site can render without styling.

## 📝 Contributing

When making changes to the site:
1. **Modify Source**: Always edit `.md` files within `build/docs/`.
2. **Promote Drafts**: Drafts from `interview-template/new_post/` must be copied into `build/docs/Blog/` before building.
3. **Rebuild**: Run `mkdocs build` to update the generated HTML.
4. **Sync Deployment Output**: Copy the matching generated `build/site` files to the repository root before deploying.
5. **Verify**: Check the root-served page and confirm the referenced CSS/JS assets return `200`.

For more detailed instructions on the development workflow, refer to [AGENTS.md](./AGENTS.md).
