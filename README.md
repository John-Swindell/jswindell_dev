# John Swindell's Portfolio Website

Source for [jswindell.dev](https://jswindell.dev), a personal portfolio built with Hugo and deployed through Cloudflare Pages.

## Technology

- **Static site generator:** Hugo Extended (`v0.147.0` or newer recommended)
- **Theme:** [hugo-profile](https://github.com/gurusabarish/hugo-profile), pinned as a Git submodule
- **Customizations:** Project-owned template overrides in `layouts/` and site configuration in `hugo.yaml`
- **Deployment:** Cloudflare Pages, deployed from the `main` branch

## Local development

Clone the repository and its theme:

```bash
git clone --recurse-submodules https://github.com/John-Swindell/jswindell_dev.git
cd jswindell_dev
```

If the repository is already cloned but the theme directory is empty, initialize it with:

```bash
git submodule update --init --recursive
```

Start the development server:

```bash
hugo server -D
```

Then open [http://localhost:1313](http://localhost:1313).

## Production build

```bash
hugo --gc --minify
```

The generated site is written to `public/`. Cloudflare Pages settings are managed outside this repository.

## Project structure

- `hugo.yaml` — homepage content and site configuration
- `content/blogs/` — project write-ups and blog posts
- `layouts/` — local template and component overrides
- `static/` — images, downloadable files, and other static assets
- `themes/hugo-profile/` — pinned upstream theme submodule

When adding raster images, prefer WebP and avoid committing unused source exports. Keep full-resolution diagrams only when visitors need to open or inspect them at that resolution.

## Contact

[LinkedIn](https://www.linkedin.com/in/john-swindell/) · [john@jswindell.dev](mailto:john@jswindell.dev)
