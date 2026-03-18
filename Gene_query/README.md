Promoter Co-binding Analysis with ReMap 2022

OVERVIEW

This project identifies transcription factors (TFs) that co-bind the promoter regions of two genes using the ReMap 2022 ChIP-seq catalog.

The script performs:

Promoter definition

Enrichment analysis

Overlap detection

Identification of shared TF binding

Ranking of TFs

Visualization of results

REQUIREMENTS

Install required R packages:

install.packages(c("ggplot2", "dplyr", "stringr", "tidyr"))
BiocManager::install(c("GenomicRanges", "ReMapEnrich"))

Load libraries:

library(ReMapEnrich)
library(GenomicRanges)
library(ggplot2)
library(stringr)
library(dplyr)
library(tidyr)

INPUT DATA

ReMap 2022 catalog (BED file)

Set the path in the script:

catalog_bed <- "C:/PathToReMapCatalog.bed"

Download from:
http://remap.univ-amu.fr/

ANALYSIS WORKFLOW

DEFINE PROMOTER REGIONS

Promoters are defined as:

2000 bp upstream of TSS

500 bp downstream of TSS

LOAD REMAP CATALOG

remapCatalog <- bedToGranges(catalog_bed)

ENRICHMENT ANALYSIS

enrichment(promoter, remapCatalog, byChrom = TRUE)

FIND OVERLAPS

hits <- findOverlaps(promoter, remapCatalog)

Extract:

Genomic coordinates

Dataset IDs

Scores

TF names (parsed from dataset ID)

IDENTIFY SHARED TFs

shared_datasets <- intersect(datasets_gene1, datasets_gene2)

RANK TF BINDING

For each dataset:

Keep highest scoring peak per promoter

Compute minimum score:

min_score = min(gene1_score, gene2_score)

Rank in descending order

OUTPUTS

Files generated:

SharedPeaks.txt

All peaks from shared TF datasets

RankedExperiments.txt

One row per TF experiment

Includes scores and genomic locations

VISUALIZATION

A grouped bar plot is generated showing:

Top 10 TF experiments

Scores for both promoters

NOTES / KNOWN ISSUES

Some variables are overwritten (e.g., gene1_best, df_gene1)

gene2 promoter currently uses incorrect coordinates (uses gene1_tss)

intersect() is applied to the same dataset instead of gene1 vs gene2

These should be corrected for accurate results.

FUTURE IMPROVEMENTS

Support multiple genes

Add statistical filtering

Integrate motif analysis

Automate plotting and reporting

AUTHOR