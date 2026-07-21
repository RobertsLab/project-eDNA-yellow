# `02.00-qiime2` — Results summary

Headline numbers extracted from the QIIME 2 `.qzv` visualizations in this
directory (produced by [`../../code/02.00-qiime2.Rmd`](../../code/02.00-qiime2.Rmd)).
Open any `.qzv` at [view.qiime2.org](https://view.qiime2.org) for the full
interactive version.

## Denoising & read retention (`deblur-stats.qzv`)

| Metric | Value |
|---|---|
| Samples entering deblur | 54 |
| Reads entering deblur (post quality-filter) | 9,887,009 |
| Reads retained (hit reference) | 770,457 |
| **Overall retention** | **~7.8%** |
| Samples with 0 reads retained | 4 |

> [!WARNING]
> Retention is low (~7.8%). `deblur denoise-other` is **reference-based**: it
> keeps only reads matching the SILVA reference at the 150 bp trim length and
> discards the rest. Combined with per-sample read loss, this leaves a small
> fraction of input reads. Worth reviewing whether the trim length, reference,
> or the paired-end/forward-read handling (see the roadmap note in the root
> README) is costing usable data. Raw input depth is also very uneven
> (min 12 reads, max 992,895 reads per sample).

## Feature table (`table.qzv`)

| Metric | Value |
|---|---|
| Samples with features | 50 |
| Features (ASVs) | 706 |
| Feature length | 150 bp (all features; `rep-seqs-deblur.qzv`) |
| Total frequency | 770,457 |
| Frequency per sample | min 26 · median 7,962 · mean 15,409 · max 100,476 |
| Frequency per feature | min 10 · median 45.5 · mean 1,091 · max 335,188 |

The per-feature distribution is highly skewed — a single feature accounts for
335,188 reads (~44% of all reads).

## Taxonomy (`taxonomy.qzv`, `taxa-bar-plots.qzv`)

Classification against the SILVA 138 99% naïve-Bayes classifier. Of 706
features: 691 assigned to **Eukaryota**, 1 to Bacteria, 14 unassigned —
consistent with a eukaryote-targeted assay (marker TODO; see root README).

**Community composition, abundance-weighted (phylum, level 2):**

| Taxon | % of reads |
|---|---|
| _Annelida_ | 52.3% |
| Eukaryota (unresolved below domain) | 28.1% |
| _Mollusca_ | 3.4% |
| _Nemertea_ | 3.0% |
| _Cercozoa_ | 2.2% |
| _Cnidaria_ | 1.8% |
| _Chlorophyta_ | 1.5% |
| _Lophotrochozoa_ | 1.4% |
| _Prymnesiophyceae_ (haptophytes) | 1.4% |
| _Arthropoda_ | 0.9% |

**Site 1 vs. Site 5 (abundance-weighted phylum):**

| | Site 1 (27 samples, 338,941 reads) | Site 5 (23 samples, 431,516 reads) |
|---|---|---|
| Dominant phylum | _Annelida_ 23.9% | _Annelida_ 74.6% |
| Unresolved Eukaryota | 43.2% | 16.4% |
| Notable | Nemertea 6.4%, Mollusca 5.5%, Cnidaria 3.4% | Cercozoa 1.9%, Mollusca 1.7% |

Site 5 is strongly dominated by annelids; Site 1 is more even and has a larger
fraction of reads unresolved below the domain level. (Sample counts here are
the 50 samples that retained features, not the original 54.)

> [!NOTE]
> These are relative-abundance summaries; a formal diversity analysis
> (rarefaction, alpha/beta diversity, statistical Site 1 vs. Site 5 comparison)
> is not yet part of the pipeline — see the roadmap in the root README.

---

*Numbers current as of the committed `.qzv` files. Re-derive by unzipping each
`.qzv` (they are zip archives) and reading `data/index.html` /
`data/level-2.csv`.*
