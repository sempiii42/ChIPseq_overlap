\# 🧬 Promoter Co-binding Analysis with ReMap 2022
## 📖 Overview

This project identifies transcription factors (TFs) that co-bind the promoter regions of two genes using the ReMap 2022 ChIP-seq catalog.

The workflow includes:

Promoter definition

Enrichment analysis

Overlap detection

Identification of shared TF binding

Ranking of TF binding strength

Visualization of results

\## ⚙️ Requirements
Install required R packages
install.packages(c("ggplot2", "dplyr", "stringr", "tidyr"))
BiocManager::install(c("GenomicRanges", "ReMapEnrich"))
Load libraries
library(ReMapEnrich)
library(GenomicRanges)
library(ggplot2)
library(stringr)
library(dplyr)
library(tidyr)
## 📂 Input Data

ReMap 2022 catalog (BED format)

Set the path in your script:

catalog\_bed <- "C:/PathToReMapCatalog.bed"

Download from:
http://remap.univ-amu.fr/

\## 🔬 Analysis Workflow

1. Define Promoter Regions

Promoter regions are defined relative to the transcription start site (TSS):

2000 bp upstream

500 bp downstream

2. Load ReMap Catalog
remapCatalog <- bedToGranges(catalog\_bed)
3. Perform Enrichment Analysis
enrichment(promoter, remapCatalog, byChrom = TRUE)
4. Find TF Binding Overlaps
hits <- findOverlaps(promoter, remapCatalog)

Extract:

Genomic coordinates (chr, start, end)

Dataset IDs

Binding scores

TF names (parsed from dataset ID)

5. Identify Shared TF Datasets
shared\_datasets <- intersect(datasets\_gene1, datasets\_gene2)

This step identifies TFs that bind both promoter regions.

6. Rank TF Binding Strength

For each TF dataset:

Keep the highest-scoring peak per promoter

Compute:

min\_score = min(gene1\_score, gene2\_score)

Rank datasets by descending min\_score

This ensures strong binding in both promoters.

\## 📊 Outputs

1. SharedPeaks.txt

All peaks from TF datasets shared between promoters

2. RankedExperiments.txt

One row per TF dataset

Includes:

Scores for both promoters

Genomic coordinates

Ranking metric (min\_score)

\## 📈 Visualization

A grouped bar plot is generated showing:

Top 10 TF datasets

Binding scores for both promoter regions

\## ⚠️ Notes / Known Issues

Some variables are overwritten (e.g., gene1\_best, df\_gene1)

gene2 promoter is incorrectly defined using gene1\_tss

intersect() is applied to the same dataset instead of gene1 vs gene2

➡️ These should be corrected to ensure accurate results.

\## 🚀 Future Improvements

Support multiple genes

Add statistical significance filtering

Integrate motif enrichment analysis

Automate reporting and figure export

\## 👤 Author

Sem Peijnenborgh
Meduni Graz

