# `project-eDNA-yellow/output`

Output directory for analyses. Each subdirectory name matches the `.Rmd` file
that produced it in [`../code`](../code).

## Subdirectories

### `01.00-trimming-fastp-fastqc-multiqc/`

Products of [`01.00-trimming-fastp-fastqc-multiqc.Rmd`](../code/01.00-trimming-fastp-fastqc-multiqc.Rmd):

| Pattern | Description |
|---|---|
| `Site*_L001_R[12]_001.fastq.gz` | Trimmed, QIIME 2-renamed paired-end reads (108 files = 54 samples × 2). *Large — see hygiene note in [`../data/README.md`](../data/README.md).* |
| `*.fastp-trim.report.html` / `.json` | Per-sample fastp trimming reports. |
| `*-fastp.stderr` | fastp run logs. |
| `*_fastqc.html` | Post-trim FastQC reports. |
| `*.md5` | Checksums for the trimmed FastQs. |
| `multiqc_report.html` | Aggregated MultiQC report for the trimmed reads. |

### `02.00-qiime2/`

Products of [`02.00-qiime2.Rmd`](../code/02.00-qiime2.Rmd). `.qza` = QIIME 2
data artifact; `.qzv` = QIIME 2 visualization. **`.qzv` files do not render on
GitHub** — download and open at [view.qiime2.org](https://view.qiime2.org).

| File | Description |
|---|---|
| `deblur-stats.qza` / `.qzv` | Per-sample read counts through each deblur denoising step. |
| `demux-paired-end-filter-stats.qza` | Stats from the initial quality-score filter. |
| `table-deblur.qza` | Feature (ASV) × sample abundance table after denoising. |
| `table.qzv` | Interactive summary of the feature table (per-sample depth, feature frequencies). |
| `rep-seqs-deblur.qza` / `.qzv` | Representative sequences for each feature. |
| `taxonomy.qza` / `.qzv` | SILVA naïve-Bayes taxonomic assignments per feature. |
| `taxa-bar-plots.qzv` | Stacked taxonomic bar plots by sample/site — the main results visualization. |

📊 **A written results summary** (read retention, feature counts, dominant taxa
by site) is in [`02.00-qiime2/README.md`](02.00-qiime2/README.md).

## Convention

All outputs from a script should be written to a directory here whose name
(minus the file suffix) matches the script, per [`../code/README.md`](../code/README.md).
