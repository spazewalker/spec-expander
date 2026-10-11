# Flavor: Research

## Dimensions to cover
- **Question:** the precise research question or hypothesis, and why it matters (what decision or next step it informs).
- **Type:** literature review, empirical experiment, replication, benchmark study, survey, or exploratory investigation.
- **Scope:** time window, fields/venues, inclusion and exclusion criteria, depth (overview vs exhaustive).
- **Evidence standard:** peer-reviewed only vs preprints and blogs allowed; how claims are verified; how uncertainty is marked.
- **Method (empirical work):** variables, baselines, ablations, datasets, compute budget, number of seeds, statistical tests.
- **Output:** report, annotated bibliography, comparison table, paper draft, slides; length and audience.

## Common defaults ([pattern], override from profile/project)
- Every factual claim carries a source link; unverified claims are flagged explicitly.
- Prefer primary sources (papers, official repos and docs) over summaries.
- Empirical claims are reported with the conditions under which they hold (hardware, dataset, baseline).
- Experiments: a strong baseline, at least 3 seeds, ablations for each claimed component, results with variance.
- Separate what is known, what is contested, and what is open.

## Typical checks
| Requirement | Check |
|---|---|
| Coverage | the list of key works/approaches the user expects is present; search strategy stated |
| Accuracy | spot-check that cited numbers match the source |
| Rigor (experiments) | baselines, seeds, ablations, and statistical tests are present |
| Reproducibility | code, configs, and data versions recorded |
| Usefulness | ends with implications or next steps tied to the original motivation |

## Questions that usually matter
- What will this research be used for (decision, paper, curiosity, build plan)?
- How deep and how recent? Any must-include works or sources to exclude?
- Is speed or rigor the priority?
- For experiments: what compute and time budget is available?
