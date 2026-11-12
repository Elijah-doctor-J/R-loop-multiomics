# R-loop multi-omics public analysis package

This package contains the analysis workflow, complete 32-gene curation table, intermediate result tables and compact processed inputs for the IJMS revision **Multi-omic analysis prioritizes an R-loop-associated gene program with prominent microglial signals in focal cerebral ischemia–reperfusion injury**.

## Download and extract the package

Download all three archives from the same repository commit and extract them into the SAME empty directory. The extracted project root must contain `scripts/`, `data/`, `docs/`, `requirements.txt`, and `README.md` side by side. Python reads the losslessly compressed `.csv.gz` tables directly.

- [Code and documentation](Batch_1_Code_and_Docs.zip)
- [Main result tables](Batch_2_Main_Result_Tables.zip)
- [Supporting data](Batch_3_Supporting_Data.zip)

The detailed documents below are inside the archives and become available after extraction.

## What is included

- `data/curated_gene_list/curated_32_rloop_genes.csv`: the complete 32-gene curation table requested by the reviewer.
- `data/main_tables/`: the cohort-level inputs and result tables used for the manuscript figures, Supplementary Tables S1–S8, and the final eight-dimension prioritization.
- `data/revision_R4_tables/`: the additional meta-analysis, sample-level single-cell checks, composition, lineage, and control-analysis tables added during revision.
- `data/figure_source/`, `data/reference/`, and `data/checkpoints/`: compact processed inputs needed by the retained scripts; large CSV inputs are losslessly stored as `.csv.gz`.
- `scripts/`: analysis, source-export, revision, and figure-generation code. Analysis outputs use a project-relative `outputs/` directory; download helpers use `data/external/`.
- `docs/`: dataset provenance, calculation definitions, figure/table map and checksums.

One final Figure 1 preview is retained at `outputs/figures/Figure1/Figure1_study_design_workflow.png`. Other generated figures, manuscript files, reviewer correspondence, old result versions, duplicate spreadsheets, internal audit notes, and duplicated public HiChIP BED files are excluded. Public raw data and large source objects are identified in `docs/DATA_SOURCES.md`; retained `.csv.gz` files preserve the supplied CSV values losslessly.

## Reproduce the supplied tables

Run from this repository root with Python 3.11+ (packages in `requirements.txt`) and the R/Bioconductor packages listed in the individual R script headers and installation helpers. The compact supplied tables are the default inputs; raw-data downloads are optional for a full upstream rebuild.

```text
python scripts/source_exports/03_recompute_singlecell_de.py
python scripts/pipeline/22_gse225948_corrected_pseudobulk.py
Rscript scripts/pipeline/16b_module_trait_statistics.R
python scripts/revision_R4/R4_1_cross_cohort_meta.py
python scripts/revision_R4/R4_1b_meta_sensitivity.py
Rscript scripts/revision_R4/r2_5b_go_orgdb_species_rerun.R
python scripts/revision_R4/r2_5_enrichment_background.py
python scripts/revision_R4/r2_6_control_signatures.py
python scripts/revision_R4/r2_6b_crosscohort_null.py
python scripts/pipeline/23_figure5_5_rloop_potential.py
python scripts/pipeline/21_figure7_integration.py
```

The GSE225948 script uses full-matrix library-size normalization and sample-level analysis. With the manuscript rule of at least ten recovered cells per pooled sample library × cell type, the recomputation has 288/310 (92.9%) same-direction Day-2 comparisons and 272/310 (87.7%) Day-14 comparisons; twelve microglial genes attain the minimum exact p-value 0.0286, while the minimum BH-adjusted p-value is 0.0738 and no primary MW comparison is FDR-significant. GSE225948 has four pooled sample libraries per group; these libraries are the units for the sample-level comparisons. Figure 5F right uses sample-library mean-expression differences. Per-cell results are exploratory. The WGCNA checkpoint contains 24 GSE137482 animals; module–trait p-values use df=22. Meta-analysis uses squared standard errors as sampling variances and retains non-estimable rows as NA. HiChIP contacts are descriptive because only one released library is available per condition.

The exploratory transcription–accessibility-based R-loop susceptibility proxy uses 26 complete cases. Ddx19 and five genes without an observed GSE163614 bulk estimate (Fancm, Fancd2, Top1, Top1mt and Pif1) are excluded from this proxy only. The curation and final integration still cover all 32 genes; missing molecular measurements are not imputed as zero in the proxy. The reported Δ is centered on the specified cross-gene min–max midpoint, not an actual Sham measurement. Its pooled-ATAC feature is distinct from the per-nucleus contrast in Figure 7; see `docs/DATA_DICTIONARY.md`.

For the exact figure-to-script and table-to-script mapping, see `docs/FIGURE_MAP.md` and `docs/DATA_DICTIONARY.md`.

## GO background analyses

The main species-specific GO BP rerun for the three animal cohorts is generated by `scripts/revision_R4/r2_5b_go_orgdb_species_rerun.R`, using rat or mouse OrgDb annotations and default/detected backgrounds. Its complete results are combined in `data/revision_R4_tables/GO_orgdb_species_all_results.csv.gz`; significant-term counts are in `GO_orgdb_species_summary.csv`. The separate approximate human-annotation sensitivity is `GO_background_sensitivity_all_results.csv.gz`, with key terms in `R2_5_GO_universe_comparison_keyterms.csv`. Both pipelines use gene-set sizes 10–500 within their respective backgrounds. Their annotations, gene mapping and term universes differ, so their counts and adjusted p-values must not be interchanged. Scripts write to `outputs/` and do not overwrite supplied snapshots. Rights and third-party terms are described in `RIGHTS.md`.

## Interpretation and versioning

GSE137482 is a permanent-MCAO mouse study with age-matched intact controls, sampled three days after occlusion without reperfusion; its age + group model uses all 24 animals. GSE22255 contains nonacute PBMC samples collected at least six months after stroke, whereas GSE16561 contains acute whole blood. These cohorts are not matched in injury timing or tissue composition.

The DDX21 rationale is contextual Tier 2 evidence for transcription and ribosomal-RNA processing, with an RNA-dependent interaction with XRN2. The curation table does not assert a DDX21–METTL3–XRN2 mechanism. Composite scoring and its numerical inputs are unchanged by these descriptive clarifications. Figure 9 plotting includes all six highest-scoring genes, including the fifth-rank tie.

The susceptibility proxy uses 26 complete cases; curation and composite prioritization retain 32 genes. Legacy filenames and column names containing `potential` are retained for code compatibility and refer to the susceptibility proxy, not a validated measurement of R-loop abundance. The intrinsic and RNase H1 components cancel in the reported midpoint-centered Δ.

Only the final Figure 1 PNG is bundled; other figures can be regenerated from their scripts and source tables. The extracted-file checksum manifest is `docs/SHA256SUMS.txt`. Verify archive hashes against `UPLOAD_SHA256SUMS.txt` before extraction. Cite the deposited repository version that contains the files used for the manuscript.
