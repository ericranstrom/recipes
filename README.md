# Recipes

A personal recipe collection, published at **https://ericranstrom.github.io/recipes/**.

Built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and deployed to GitHub Pages by GitHub Actions on every push to `main`.

## Run locally

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve        # live preview at http://127.0.0.1:8000/recipes/
mkdocs build --strict   # same check CI runs
```

## Layout

- `docs/recipes/<category>/`: recipes (mains, sides, salads, breakfast, desserts)
- `docs/components/`: reusable pieces (dressings, sauces, etc.) that recipes link to
- `templates/recipe.md`: the template every recipe and component follows
- `CLAUDE.md`: conventions for maintaining the repo, including the tag list
