# `project-eDNA-yellow/data`

Input data for the pipeline: reference databases, sample metadata, and raw-read
QC reports.

## Contents

| Path | Tracked? | Description |
|---|---|---|
| `silva-138-99-seqs.qza` | ✅ (93 MB) | SILVA 138 99% reference sequences (QIIME 2 artifact) used as the reference for `deblur denoise-other` in `02.00`. |
| `silva-138-99-seqs.qza.md5` | ✅ | MD5 checksum for the reference sequences. |
| `silva-138-99-nb-classifier.qza` | ❌ (git-ignored) | Pre-trained SILVA 138 99% naïve-Bayes classifier for taxonomy. Download from the [QIIME 2 data resources](https://library.qiime2.org/data-resources#naive-bayes-classifiers). Not tracked because of its size. |
| `raw-fastqs/metadata.tsv` | ✅ | Sample metadata (see dictionary below). |
| `raw-fastqs/*_fastqc.html` | ✅ | Per-sample FastQC reports for the raw reads (output of `00.00`). |
| `raw-fastqs/multiqc_report.html` | ✅ | Aggregated MultiQC report for the raw reads. |
| Raw `*.fastq.gz` | ❌ (not in git) | Raw reads live on the lab server: <https://owl.fish.washington.edu/nightingales/eDNA-yellow_island-2023/> |

## `metadata.tsv` dictionary

QIIME 2-formatted sample metadata (tab-separated). 54 samples.

| Column | Description |
|---|---|
| `id` | Sample identifier, e.g. `Site1a`, `Site5bb`. Matches the `SiteN...` token in the FastQ filenames. |
| `site` | Collection site label, e.g. `1a`, `5bb`. First character is the site number (1 or 5). |

Sample counts: **Site 1 = 28**, **Site 5 = 26**.

> [!NOTE]
> **TODO — metadata enrichment.** The current metadata has only `id` and `site`.
> Consider adding collection date, latitude/longitude, replicate ID, sample
> volume filtered, and any environmental covariates. Note also that 60 raw
> sample sets were sequenced but 6 empty ones were dropped during `00.00`/`01.00`,
> leaving 54 — document which were removed and why.

## Naming

`00.00`/`01.00` rename samples from the raw
`Yell_July18_Site1f_S92_L001_R[12]_001.fastq.gz` form to the QIIME 2-compliant
`Site1f_S92_L001_R[12]_001.fastq.gz` form (Casava one-eight single-lane format)
before import in `02.00`.

---

## Repository-hygiene note (data weight)

The repository currently tracks ~1.5 GB, dominated by intermediate data:

- 108 trimmed `*.fastq.gz` in `output/01.00-trimming-fastp-fastqc-multiqc/`
- ~163 FastQC HTML reports across `data/raw-fastqs/` and `output/01.00-...`

`.gitignore` lists `*.fastq.gz`, but these files were committed **before** the
ignore rule was added, so git still tracks them — an ignore rule does not
untrack files already in history.

**Recommended cleanup (discuss before running — these rewrite the index):**

```bash
# Stop tracking the trimmed FastQs going forward (keeps them on disk):
git rm --cached output/01.00-trimming-fastp-fastqc-multiqc/*.fastq.gz
git commit -m "Untrack trimmed FastQs (available on lab server)"
```

Trimmed reads and raw reads can be regenerated (`01.00`) or re-downloaded from
the owl server, so they do not need to live in git. If shrinking the packed
history (not just future commits) is important, a history rewrite with
[`git filter-repo`](https://github.com/newren/git-filter-repo) is required —
coordinate with collaborators first, since it changes every commit hash.

Keep in git: MultiQC summary reports, `metadata.tsv`, the SILVA reference
sequences, and the small `02.00-qiime2` `.qza`/`.qzv` results.
