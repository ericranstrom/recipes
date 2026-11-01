# CLAUDE.md

Maintenance guide for this repo: a **public** recipe website built with MkDocs Material, published at https://ericranstrom.github.io/recipes/ via GitHub Actions (`.github/workflows/deploy.yml`, runs on push to `main`).

## Layout

```
docs/
  index.md                 home page: links to categories and tags
  tags.md                  auto-generated tag index (do not hand-edit the list)
  components/              reusable building blocks (flat folder)
    index.md               grouped list: Dressings, Sauces, Roasted Vegetables, Base Proteins, Grains
  recipes/
    mains/ sides/ salads/ breakfast/ bread/ desserts/
      index.md             alphabetical list of the recipes in that category
templates/recipe.md        the template for every recipe AND component
overrides/partials/content.html   shows servings/times/source from front matter under the title
mkdocs.yml                 config, including the enforced tag list (tags_allowed)
```

## Adding a recipe ("add this recipe")

Input may be a URL, a photo, pasted text, or a rough description. Then:

1. **Normalize it into `templates/recipe.md`.** Fill every front matter field:
   - `title`: Title Case, plain name (e.g. "Lemon Chicken").
   - `tags`: from the vocabulary below only.
   - `servings`, `active_time`, `total_time`: e.g. `4`, `20 min`, `1 hr 15 min`. Estimate if the source doesn't say.
   - `source`: the URL, or `family`, or `original`.
   - `date_added`: today's date, `YYYY-MM-DD`, in **Pacific time** (`TZ=America/Los_Angeles date +%F`). The container clock is UTC, which runs ahead in the evening.
2. **Body sections, in this order:** one-line description (no heading), `## Ingredients`, `## Steps`, `## Notes & Variations`, `## Cook Log`. Don't add an H1; the title comes from front matter.
   - Ingredients: group with `###` subheadings when there are multiple parts (e.g. Chicken / Sauce / To serve).
   - Steps: numbered, **one action per step**. Split compound steps.
   - Cook Log: start with an empty list if I haven't cooked it yet, or a first entry if I have.
3. **Pick the category folder:** `mains`, `sides`, `salads`, `breakfast`, `bread`, or `desserts`. Ask only if it's truly ambiguous.
4. **Filename:** kebab-case from the title, e.g. `docs/recipes/mains/lemon-chicken.md`.
5. **Add a line to that category's `index.md`**, keeping it alphabetical; remove the `*Nothing here yet.*` placeholder if present.

### Web sources
- **Paraphrase the instructions in your own words.** Do not copy blog text, headnotes, or step wording verbatim. Ingredient lists (quantities and items) are facts and can be transcribed.
- Always keep the source URL in `source`.
- Skip the life story; the one-line description is ours.

### Components
- Pull reusable pieces (dressings, sauces, spice blends, roasted veg, base proteins, grains) into `docs/components/<kebab-name>.md` when they'd plausibly be used by another recipe. Use the same template and tag them `component`.
- Before creating one, check whether an existing component already covers it, and link to that instead.
- In the recipe, link to it with a relative link instead of repeating it, both in Ingredients and in the step that makes it:
  - from `docs/recipes/<category>/x.md`: `[Component Name](../../components/component-name.md)`
- Add the component to the right group in `docs/components/index.md` (alphabetical).

## Tag vocabulary

This list is fixed. It's also enforced by `tags_allowed` in `mkdocs.yml`: an unlisted tag fails the build. **Only add a new tag if nothing here fits**, and when you do, add it both here and in `mkdocs.yml` in the same commit.

All tags are lowercase kebab-case. Give each recipe the tags that apply from each group, typically: protein (or diet), cuisine, meal type, effort, and `make-ahead` if relevant.

| Group | Tags |
|---|---|
| Protein | `chicken`, `beef`, `pork`, `lamb`, `turkey`, `fish`, `shellfish`, `eggs`, `tofu`, `beans` (includes lentils and chickpeas) |
| Diet | `vegetarian`, `vegan`, `gluten-free`, `dairy-free` |
| Cuisine | `american`, `mexican`, `italian`, `french`, `mediterranean`, `middle-eastern`, `indian`, `chinese`, `japanese`, `korean`, `thai`, `vietnamese` |
| Meal type | `breakfast`, `lunch`, `dinner`, `side`, `salad`, `dessert`, `snack`, `component` |
| Effort | `weeknight` (about 45 min or less, low fuss), `project` (long, multi-stage, or weekend cooking) |
| Planning | `make-ahead` (holds well or is meant to be prepped ahead) |
| Occasion | `holiday` (holiday and special-occasion dishes) |

## Units

- US measurements (cups, tbsp, tsp, oz, lb, °F).
- **Baking:** add grams in parentheses for dry ingredients and fats, e.g. `2 cups (250 g) all-purpose flour`, `1/2 cup (113 g) butter`.
- Use `°F` for oven temps.

## Cook Log updates

When I report how a cook went:
- Add a dated one-liner at the **top** of `## Cook Log` (newest first): `- 2026-10-31: halved the rice, needed more lemon`.
- Dates are Pacific time. If I name a day ("Halloween", "last Sunday"), use that date.
- Only change the recipe itself if I say the change is permanent (e.g. "make that the default"). Otherwise it lives in the log, or in Notes & Variations if it's a variation worth keeping.

## Privacy

This repo and site are **public**. Never include names, photos, ages, health details, locations, or other personal info about family members. A source of `family` is fine; "Grandma Jane's" is not. Keep notes general and name-free (e.g. "a hit with kids").

## Before every commit

1. Run `mkdocs build --strict` and fix every warning or error (broken links, unknown tags, etc.).
   - Setup if needed: `python3 -m venv .venv && .venv/bin/pip install -r requirements.txt`
2. Commit with a clear message, e.g.:
   - `Add recipe: lemon chicken`
   - `Add component: chimichurri`
   - `Update recipe: lemon chicken (more garlic, cook log)`
   - `Add tag: korean`
3. Don't commit `site/` or virtualenvs (they're in `.gitignore`).
4. **Work directly on `main` and push to `main`.** No feature branches or PRs for this repo, even if the session was started on another branch. Pushing to `main` deploys the site automatically.

## Reference recipe

`docs/recipes/mains/beef-chuck-chili.md` is a real recipe and the reference for formatting. For long recipes, it shows how to split Steps into `###` phases (e.g. "Day 1" / "Day 2") with numbering that continues across phases: start the second list at the next number.
