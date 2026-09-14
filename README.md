<p align="center">
  <img src="_meta/necrophagous-insects-pineywoods-2025-banner.png" width="800">
</p>

This repository houses the analysis and metadata behind the following publication:
> Grigsby A, Villarreal W, Rhodes T, Seitz VA, Schreiber M, Nieciecki V,
> Deel H, Otto K, Creighton JC, Bates ST, Bucheli SR, Metcalf JL.
> *Host-specific microbial partnerships with poorly characterized bacteria
> underpin necrophagous insect ecology in a deciduous subtropical forest.*
> In preparation for Sustainable Microbiology (Oxford University Press).

It takes the QIIME 2 exports in `data/` and uses them to produce every figure and table in the manuscript + it's supplement. Raw reads and processed exports are available separately.

Adult necrophagous insects were caught on decomposing rabbit carcasses at the Pineywoods Environmental Research Laboratory, Huntsville, TX, in spring 2025. Each specimen yielded an exogenous (distilled-water wash of the body surface) and endogenous (surface-sterilized homogenate) microbiota sample. The beetle *Necrodes surinamensis* was dissected into abdomen and head-thorax. Flies were extracted whole. Libraries target the 16S rRNA V4 region (515F/806R as per the EMP protocol) and were sequenced on an Illumina MiSeq (2x251 bp) across two runs (`sr64` and `sr65`).

## Data availability
| Data | Location |
|---|---|
| Raw reads (177 demultiplexed libraries) | ENA study `PRJEB______` |
| Qiita study | `______` |
| Feature table, taxonomy, tree, metadata | `data/qiime2-exports/` |
| Archived intermediate results | `data/derived/` |

## Files
`data/qiime2-exports/` houses the output of the QIIME 2 2026.4 pipeline used.
| File | Content |
|---|---|
| `metadata.tsv` | Sample metadata |
| `feature-table.biom`, `feature-table.tsv` | ASV counts after mitochondria and chloroplast removal |
| `rep-seqs.fasta` | ASV sequences |
| `taxonomy.tsv` | Greengenes2 2024.09 assignments |
| `taxonomy-corrected.tsv` | Taxonomic assignments with a small set of implausible assignments corrected by BLAST against the NCBI 16S database |
| `tree.nwk` | SEPP insertion tree on the Greengenes2 2022.10 backbone |
| `dada2-stats-SR64.tsv`, `dada2-stats-SR65.tsv` | Read counts through DADA2 (per run) |
| `feature-table-genus.tsv` | Genus-level table |

The pipeline used EMP paired-end demultiplexing with Golay error correction (reverse-complemented barcodes and mapping barcodes), DAD2 `denoise-paired` per run w/ truncation at `230` bp forward and `180` bp reverse, consensus chimera removal, etc. Tables were merged across runs and taxonomy was assigned using the naive-Bayes classifier fitted to the Greengenes2 2024.09 V4 backbone. SEPP fragment insertion into the Greengenes2 2022.10 backbone was used to generate a tree. Mitochondria and chloroplasts were removed. Reagent contaminants are removed in `r` with `decontam` using the controls.

## Environment
Analyses were run locally on an M3 MacBook Pro with 12 cores and 18 GB of memory. We used `r` for every analysis script and QIIME 2 2026.4 (`qiime2-amplicon-2026.4`) for raw sequence data processing.

The following `r` packages were used in particular:
- `phyloseq` v1.54.0
- `vegan` v2.7.3
- `ANCOMBC` v2.12.0
- `SpiecEasi` v1.1.2
- `igraph` v2.1.4
- `ComplexHeatmap` v2.26.1
- `ranger` v0.18.0
- `pROC` v1.19.0.1
- `decontam` v1.30.0
- `picante` v1.8.2
- `ggplot2` v4.0.2
