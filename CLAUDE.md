# Causal Inference Knowledge Vault

## Project Context
Personal knowledge vault focused exclusively on causal inference methods, identification strategies, and estimation. Uses Obsidian with MOC (Map of Content) hub pages. Organized following the pedagogical structure of Facure's "Causal Inference in Python" with additional coverage from econometrics literature.

## Note Conventions
- **Frontmatter**: Every note MUST have: title, aliases (list), tags (list), updated (YYYY-MM-DD)
- **Structure**: # Title → > [!summary] → ## sections → optional ## Code snippets → ## Related notes
- **Filenames**:
  - Acronyms with known expansions: `Full Name (ABBR).md` in Title Case
    (e.g., `Inverse Probability Weighting (IPW).md`, `Average Treatment Effect (ATE).md`)
  - Formal methods/proper nouns: Title Case (e.g., `Callaway–Sant'Anna estimator.md`)
  - Generic concepts: lowercase (e.g., `propensity score.md`, `parallel trends.md`)
  - MOC hubs: `Topic Name (MOC).md`
- **Aliases**: Add all common abbreviations and alternative names
- **Tags**: Use existing taxonomy: moc, causal-inference, identification, potential-outcomes, dags, propensity-score, did, iv, rdd, synthetic-control, heterogeneity, meta-learners, spillovers, sensitivity, bounds, panel-data
- **Links**: Use Obsidian wikilinks `[[Note Name]]`. Never include `.md` extension in links.
- **Math**: Use `$$...$$` for display, `$...$` for inline. Standard LaTeX notation.
- **Code blocks**: Python preferred (econml, causalml, statsmodels, linearmodels). Include only where it helps — estimator and workflow pages should have snippets; MOCs and concept pages may omit code.
- **Callouts**: `> [!summary]`, `> [!warning]`, `> [!tip]`, `> [!check]`, `> [!example]`

## When Creating a New Note
1. Check if the topic already exists (search filenames and aliases)
2. Use the standard structure (frontmatter → summary → sections → code → related)
3. Link FROM at least one MOC page
4. Link TO related concepts using wikilinks
5. Set `updated:` to today's date
6. Add to the relevant MOC's index

## Directory Structure
Flat vault — all `.md` files in root directory. No subdirectories for content.

## Reference Materials
- Facure, "Causal Inference in Python" (O'Reilly) — structural spine
- Angrist & Pischke, "Mostly Harmless Econometrics" — IV, DiD, RDD
- Imbens & Rubin, "Causal Inference for Statistics" — potential outcomes, matching
- Hernán & Robins, "Causal Inference: What If" — DAGs, g-methods, TMLE
- Cunningham, "Causal Inference: The Mixtape" — applied econometrics
