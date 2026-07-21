# project-eDNA-yellow

Environmental DNA (eDNA) metabarcoding analysis of water samples collected at
**Yellow Island** (San Juan Islands, WA). This repository contains the
bioinformatics pipeline that takes raw amplicon sequencing reads through quality
control, trimming, and taxonomic classification with
[QIIME 2](https://qiime2.org/).

> [!NOTE]
> **TODO — study description.** Fill in the biological context that cannot be
> inferred from the code: the target marker gene (e.g. 18S rRNA / COI), PCR
> primers, sampling design and dates, number of biological vs. technical
> replicates, and the overall project goal/hypothesis. Sample filenames use
> `Yell_July18_...` while the raw-read host directory is named
> `eDNA-yellow_island-2023` — confirm the actual collection year.

## Study at a glance

| | |
|---|---|
| **Location** | Yellow Island, San Juan Islands, WA |
| **Sites** | Site 1 (n = 28) and Site 5 (n = 26) — 54 samples total |
| **Assay** | eDNA amplicon metabarcoding (marker: _TODO_) |
| **Sequencing** | Paired-end Illumina, `L001_R[12]_001.fastq.gz` |
| **Reference DB** | SILVA 138 99% (`silva-138-99`) |
| **Classifier** | QIIME 2 2024.10, naïve-Bayes sklearn |

## Pipeline

Analyses run sequentially; each notebook writes to a matching directory in
[`output/`](output/). Notebooks are authored in R Markdown and knitted to
`.md` for viewing on GitHub.

```
00.00  raw reads ──► FastQC + MultiQC (raw QC)
  │
01.00  raw reads ──► fastp trim ──► FastQC + MultiQC ──► QIIME2-compliant rename
  │                                                        └► output/01.00-.../ (trimmed FastQs)
02.00  trimmed reads ──► QIIME2 import ──► quality filter ──► deblur denoise
                          ──► SILVA classify ──► taxa bar plots
                                                  └► output/02.00-qiime2/ (.qza / .qzv)
```

| Step | Notebook | Output |
|---|---|---|
| Raw-read QC | [`code/00.00-fastqc-multiqc-raw-reads.Rmd`](code/00.00-fastqc-multiqc-raw-reads.Rmd) ([md](code/00.00-fastqc-multiqc-raw-reads.md)) | MultiQC report |
| Trimming + QC | [`code/01.00-trimming-fastp-fastqc-multiqc.Rmd`](code/01.00-trimming-fastp-fastqc-multiqc.Rmd) ([md](code/01.00-trimming-fastp-fastqc-multiqc.md)) | [`output/01.00-trimming-fastp-fastqc-multiqc/`](output/01.00-trimming-fastp-fastqc-multiqc/) |
| Taxonomic classification | [`code/02.00-qiime2.Rmd`](code/02.00-qiime2.Rmd) ([md](code/02.00-qiime2.md)) | [`output/02.00-qiime2/`](output/02.00-qiime2/) |

## Viewing results

QIIME 2 `.qzv` visualizations in [`output/02.00-qiime2/`](output/02.00-qiime2/)
cannot be rendered on GitHub. Download a `.qzv` file and open it at
**[view.qiime2.org](https://view.qiime2.org)** (drag-and-drop; nothing is
uploaded to a server). Key files: `taxa-bar-plots.qzv`, `table.qzv`,
`taxonomy.qzv`, `deblur-stats.qzv`.

## Reproducing the analysis

**Software**

- R + RStudio (notebooks use `knitr`, `dplyr`, `reticulate`)
- [fastp](https://github.com/OpenGene/fastp), [FastQC](https://github.com/s-andrews/FastQC), [MultiQC](https://multiqc.info/)
- [QIIME 2 2024.10](https://docs.qiime2.org/2024.10/) in a `qiime2-amplicon-2024.10` conda/mamba environment

**Steps**

1. Clone this repo.
2. Update the path variables at the top of each notebook — e.g. `repo_dir`,
   `qiime2_conda_path`, and `qiime2_conda_env_name` in
   [`02.00-qiime2.Rmd`](code/02.00-qiime2.Rmd) — to match your machine.
3. Provide the reference data in [`data/`](data/): the SILVA reference sequences
   (`silva-138-99-seqs.qza`, tracked) and the naïve-Bayes classifier
   `silva-138-99-nb-classifier.qza` (not tracked — see [`data/README.md`](data/README.md)).
4. Knit the notebooks in order (00 → 01 → 02).

> [!NOTE]
> **TODO — reproducibility.** Add a conda environment export
> (`qiime2-amplicon-2024.10.yml`) and an R `sessionInfo()` capture so the exact
> software versions are pinned.

## Data provenance

- **Raw reads** are hosted on the Roberts Lab server, not in git:
  <https://owl.fish.washington.edu/nightingales/eDNA-yellow_island-2023/>
  ([`00.00`](code/00.00-fastqc-multiqc-raw-reads.Rmd) downloads them).
- **Sample metadata:** [`data/raw-fastqs/metadata.tsv`](data/raw-fastqs/metadata.tsv).
- See [`data/README.md`](data/README.md) for the full data inventory.

## Repository layout

```
├── code/     Analysis notebooks (.Rmd + knitted .md) and references.bib
├── data/     Reference sequences, sample metadata, raw-read QC reports
└── output/   Per-notebook outputs (directory name matches the notebook)
```

Naming and organization conventions are documented in
[`code/README.md`](code/README.md) and [`output/README.md`](output/README.md).

## Status & roadmap

The pipeline runs end-to-end and all three notebooks have been executed. Open
items:

- [ ] Fill study-description and reproducibility TODOs above.
- [x] Summarize headline results — see [`output/02.00-qiime2/README.md`](output/02.00-qiime2/README.md).
- [ ] Investigate low deblur retention (~7.8% of reads reach the feature table).
- [ ] Add downstream ecology: alpha/beta diversity and Site 1 vs. Site 5 comparison, with exported figures/tables (not just `.qzv`).
- [ ] Confirm paired-end handling in `02.00` — reads are imported as paired-end but `deblur denoise-other` operates on forward reads; verify R2 use is as intended.
- [ ] Resolve repo weight (~1.5 GB tracked; trimmed FastQs and QC HTMLs are committed) — see the hygiene notes in [`data/README.md`](data/README.md).

## Attribution

Part of the [Roberts Lab](https://github.com/RobertsLab), University of
Washington. Pipeline authored by Sam White.
