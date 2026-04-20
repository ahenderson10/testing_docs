# VectorBlox Documentation Site

Source for the VectorBlox documentation site published at https://lianvictor.github.io/testing_docs/.

The site is built with [Jekyll](https://jekyllrb.com) and the [Just the Docs](https://just-the-docs.github.io/just-the-docs/) theme. Content is organized under top-level section folders (`getting-started/`, `sdk/`, `tutorials/`, `compression/`, `hardware/`, `app-notes/`, `examples/`) and uses front-matter `parent` / `nav_order` to build the sidebar.

## Local preview

```bash
bundle install
bundle exec jekyll serve
```

The site will be available at http://localhost:4000/.

## Publishing

Pushes to `main` trigger the GitHub Pages workflow defined in `.github/workflows/pages.yml`.
