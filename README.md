# go-freedesktop docs

Source for the versioned documentation site at
**<https://go-freedesktop.github.io/docs/>**, built with
[MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and versioned
with [mike](https://github.com/jimporter/mike).

## Local preview

```sh
pip install -r requirements.txt
mkdocs serve
```

Pushes to `main` deploy automatically via `.github/workflows/docs.yml` (mike →
`gh-pages`, served at `/docs/`).

BSD-3-Clause.
