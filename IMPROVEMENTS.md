# IMPROVEMENTS.md

*Analysis date: 2026-07-11*

LEAFTOP is a published dataset repository (version 1.0, released 2022-01-13, LREC 2022 paper) containing vocabulary extracted from ~700 low-resource languages: per-language `data/<iso>-vocab.csv` + `<iso>-vocab.xlsx` + `<iso>-metadata.txt`, a `docs/language_index.csv`, and hand-checked `evaluations/*.xlsx` for ~38 languages. There is no source code in this repo — the extraction pipeline lives elsewhere. The repo is clean (no uncommitted changes) and essentially dormant since 2022; the improvements below are mostly about discoverability, reproducibility, and dataset hygiene rather than bug fixes.

## Documentation (highest priority)

- **README.md is four lines long.** For a dataset with a citable paper, expand it to include: the paper title and a link/BibTeX for the LREC 2022 paper ("LEAFTOP: A grammatically corrected list of nouns from the New Testament..."), the LDC catalog number and link (README says "see the LDC release" but gives no pointer), the data schema, the license, and a couple of usage examples.
- **Document the CSV schema.** Columns like `extract_reference_id`, `tokenisation_method_id`, `cumulative_confidence`, and `version_155_translation` are opaque. Add a `docs/schema.md` (or expand `docs/README.txt`) explaining each column, what the confidence score means and how to threshold it, and what "version_155" refers to.
- **Explain `evaluations/`.** `evaluations/README.txt` exists but the directory contains a `scratch` subdirectory and inconsistently named files (`shu.xlsx` vs `shu_rom.xlsx`, `urd_dv.xlsx`) — document the naming convention or clean up `scratch`.
- **No LICENSE file.** A dataset repo without an explicit license is legally unusable by most downstream researchers. Add one (CC-BY 4.0 or similar, consistent with the LDC release terms).

## Data hygiene

- **Drop or generate the .xlsx duplicates.** Every language ships both CSV and XLSX with identical content, roughly doubling repo size and inviting drift. Keep CSV as canonical; either delete the XLSX files or note they are generated and provide the generation script.
- **Metadata files have formatting artifacts.** e.g. `data/aai-metadata.txt` ends with a stray tab-separated number (`...Oro Province, Papua New Guinea    4510`) and double spaces ("is  written"). A quick pass over all `*-metadata.txt` files to strip these would improve machine-readability; better, convert metadata to a single structured CSV/JSON.
- **Consider Git LFS or a release tarball** if the repo is heavy to clone (hundreds of xlsx binaries); GitHub Releases with a zipped `data/` would help casual users.

## Reproducibility

- **Link the pipeline code.** The extraction code that produced this dataset is not in the repo. Add a pointer to wherever it lives (or vendor it in a `pipeline/` directory) so results are reproducible. If any of that code is Python with a `requirements.txt`, migrate it to `uv` (`pyproject.toml` + `uv add`, run via `uv run script.py`).
- **CITATION.cff is minimal.** Add `preferred-citation` with the full LREC 2022 conference-paper entry so `gh repo view`/Zenodo/GitHub's "Cite this repository" produces the paper citation, not just the repo.

## Quick wins

- Fix the commit-history smell: `04e1180 Update README.txt` vs `Update README.md` suggests a renamed file — confirm no stale `README.txt` references remain in docs.
- Add GitHub topics (low-resource-languages, nlp, dataset, bible-corpus) so researchers can find it.
- Add a `CHANGELOG.md`; if errata have been found since the 2022 release, note them even if the data itself stays frozen at 1.0.
- Consider archiving on Zenodo for a DOI, and add the DOI badge to README.

## Security

- No code, no secrets found; nothing to flag. The only exposure is the SSH-style remote in contributors' clones, which is normal.
