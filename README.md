# R-loop multi-omics public analysis package

## Download and extract the package

Download all three archives below and extract all three into the SAME empty directory. Their paths are relative to the project root; the extracted directory should contain `scripts/`, `data/`, `docs/`, `requirements.txt`, and `README.md` side by side. Large tables ending in `.csv.gz` are losslessly compressed and read directly by the supplied Python scripts.

- [Code and documentation](Batch_1_Code_and_Docs.zip)
- [Main result tables](Batch_2_Main_Result_Tables.zip)
- [Supporting data](Batch_3_Supporting_Data.zip)

Use the three archives from the same repository commit. Detailed workflow and provenance documents referenced below are inside the archives and become available after extraction.


This package contains the minimum reproducibility materials for the IJMS revision **Multi-omic analysis prioritizes an R-loop-associated gene program with prominent microglial signals in focal cerebral ischemia–reperfusion injury**.

## What is included

- `data/curated_gene_list/curated_32_rloop_genes.csv`: the complete 32-gene curation table requested by the reviewer.
- `data/main_tables/`: the cohort-level inputs and result tables used for the manuscript figures, Supplementary Tables S1–S8, and the final eight-dimension prioritization.
- `data/revision_R4_tables/`: the additional meta-analysis, sample-level single-cell checks, composition, lineage, and control-analysis tables added during revision.
- `data/figure_source/`, `data/reference/`, and `data/checkpoints/`: compact processed inputs needed by the retained scripts; large CSV inputs are losslessly stored as `.csv.gz`.
- `scripts/`: analysis, source-export, revision, and figure-generation code. Analysis outputs use a project-relative `outputs/` directory; download helpers use `data/external/`.
- `docs/`: dataset provenance, calculation definitions, figure/table map, release notes, and checksums.

Generated figures, manuscript files, reviewer correspondence, old result versions, duplicate spreadsheets, internal audit notes, and duplicated public HiChIP BED files are excluded. Public raw data and large source objects are identified in `docs/DATA_SOURCES.md`; all retained `.csv.gz` files are byte-for-byte lossless compression of the supplied CSV snapshots.

## Reproduce the supplied tables

Run from this repository root with Python 3.11+ and the R/Bioconductor packages listed in `requirements.txt` and the individual script headers. The compact supplied tables are the default inputs; raw-data downloads are optional for a full upstream rebuild.

```text
python scripts/source_exports/03_recompute_singlecell_de.py
python scripts/pipeline/22_gse225948_corrected_pseudobulk.py
Rscript scripts/pipeline/16b_module_trait_statistics.R
python scripts/revision_R4/R4_1_cross_cohort_meta.py
python scripts/revision_R4/R4_1b_meta_sensitivity.py
python scripts/pipeline/21_figure7_integration.py
python scripts/pipeline/23_figure5_5_rloop_potential.py
```

The single-cell script uses corrected full-matrix normalization and sample-level analysis. With the manuscript rule of at least ten recovered cells per animal×cell type, the retained recomputation has 288/310 (92.9%) same-direction Day-2 comparisons and 272/310 (87.7%) Day-14 comparisons; twelve microglial genes attain the minimum exact p-value 0.0286, while the minimum BH-adjusted p-value is 0.0738 and no primary MW comparison is FDR-significant. Per-cell GSE225948 results are exploratory and are not treated as independent biological replication. The WGCNA checkpoint contains 24 GSE137482 animals; module–trait p-values use df=22. Meta-analysis uses squared standard errors as sampling variances and retains non-estimable rows as NA. HiChIP contacts are descriptive because only one released library is available per condition.

For the exact figure-to-script and table-to-script mapping, see `docs/FIGURE_MAP.md` and `docs/DATA_DICTIONARY.md`. This local package is a versioned upload preparation; it does not itself create or update a GitHub commit or Zenodo DOI.


