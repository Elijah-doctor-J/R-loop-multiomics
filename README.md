# R-loop multi-omics in focal cerebral ischemia–reperfusion injury

Analysis code, curated genes and processed intermediate results accompanying
*Multi-omic analysis prioritizes an R-loop-associated gene program with prominent
microglial signals in focal cerebral ischemia–reperfusion injury* (IJMS revision).

## Contents

| Location | Contents |
|---|---|
| `data/curated_gene_list/curated_32_rloop_genes.csv` | 32 mouse/human gene pairs, functional classification, selection basis and R-loop evidence tiers |
| `data/main_tables/` | Cohort DE, enrichment, WGCNA/PPI, single-cell, accessibility, loop annotation, final scores and weight sensitivity |
| `data/revision_R4_tables/` | Meta-analysis, lineage, composition and control-analysis results |
| `data/figure_source/` | Compact expression/embedding inputs and released processed HiChIP interactions |
| `data/checkpoints/` | Small WGCNA checkpoint for the 24-sample network |
| `data/reference/` | Dataset metadata, marker panels, annotation snapshot and source records |
| `scripts/pipeline/` | Download, processing and analysis stages; one final integration implementation |
| `scripts/source_exports/` | Checkpoint export and final single-cell DE |
| `scripts/revision_R4/` | Revision analyses and panels |
| `scripts/figure_panels/` | Main-figure panels and supplementary volcanoes |
| `docs/` | Sources, statistical conventions, figure/table map, release notes and checksums |

Duplicate final images, supplementary Excel copies, historical results and
manuscript-editing materials are omitted. Their analysis data are retained in
CSV format. The 32-gene curation is included directly as a single CSV.

## Run from the supplied processed inputs

Use Python 3.11+ and R/Bioconductor for the R stages. Python dependencies:

```text
python -m pip install -r requirements.txt
python scripts/source_exports/03_recompute_singlecell_de.py
Rscript scripts/pipeline/16b_module_trait_statistics.R
python scripts/revision_R4/R4_1_cross_cohort_meta.py
python scripts/revision_R4/R4_1b_meta_sensitivity.py
python scripts/pipeline/21_figure7_integration.py
python scripts/pipeline/23_figure5_5_rloop_potential.py
python scripts/run_figures.py
```

New files go to `outputs/`. These commands do not replace the supplied `data/`
snapshots. The figure runner covers Figures 1–5 and 7–9. Figure2F is produced by
the meta script; Figure6 and revision supplementary panels use
`scripts/revision_R4/R4_plot_new_analysis_panels.py`.
See [FIGURE_MAP.md](docs/FIGURE_MAP.md) for specific output mappings.

For public-input downloads and upstream processing, follow
[pipeline/README.md](scripts/pipeline/README.md) and
[DATA_SOURCES.md](docs/DATA_SOURCES.md). Large public raw data and Seurat objects
are obtained from their source records. Downstream analysis can use the bundled
processed inputs. Preparation of this release does not certify an end-to-end
rerun from raw reads, and dependency ranges are not a historical environment lock.

## Interpretation

The 32-gene list includes direct R-loop regulators and contextual members; it is
not a list of 32 experimentally proven R-loop activities in ischemic microglia.
The 31-gene formation-potential output excludes Ddx19 and is a heuristic proxy.
Single-cell tests use cells; composition tables use animals. HiChIP scores
describe gene-window contact detection in the released Sham/Day6 libraries,
without biological-replicate or differential-contact inference.

Exact score rules, tie handling and legacy column scales are in
[DATA_DICTIONARY.md](docs/DATA_DICTIONARY.md). [RIGHTS.md](RIGHTS.md) records reuse
status. A public repository URL and archive DOI must be assigned when publishing;
this local package does not create either identifier.
