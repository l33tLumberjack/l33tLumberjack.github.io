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

## ⚙️ Development & Build Instructions

### Prerequisites

Ensure you have Python installed, then install the necessary dependencies:

```bash
pip install mkdocs-material
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

## 📝 Contributing

When making changes to the site:
1. **Modify Source**: Always edit `.md` files within `build/docs/`.
2. **Rebuild**: Run `mkdocs build` to update the generated HTML.
3. **Verify**: Check the `build/site` directory to ensure your changes are reflected correctly.

For more detailed instructions on the development workflow, refer to [AGENTS.md](./AGENTS.md).
