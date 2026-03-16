<<<<<<< HEAD
# README – p53–FoxM1 Overlap and TF Enrichment Analysis

## Overview

This R script identifies **genomic regions where p53 and FoxM1 ChIP-seq peaks overlap**, assigns the **nearest genes**, and performs **transcription factor enrichment analysis** using the ReMap transcription factor binding catalog.

The workflow performs the following steps:

1. Load p53 and FoxM1 BED peak files.
2. Expand peaks and compute overlapping regions.
3. Export overlapping loci and peak scores.
4. Assign the nearest gene to each locus using Ensembl gene annotations.
5. Perform transcription factor enrichment using the ReMap catalog.
6. Generate visualization plots of enriched transcription factors.

This workflow is useful for identifying **co-bound regulatory regions** and **potential transcriptional co-regulators** of p53 and FoxM1.

---

# Requirements

## R Packages

The script requires the following R packages:

* GenomicRanges
* ReMapEnrich
* rtracklayer
* ggplot2
* reshape2
* biomaRt

Install missing packages using:

```r
install.packages(c("ggplot2","reshape2"))

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install(c("GenomicRanges","rtracklayer","biomaRt"))
```

Install **ReMapEnrich** according to its documentation if not already available.

---

# Input Files

The script requires three main input files.

### 1. p53 ChIP-seq peaks

BED file containing p53 peak coordinates.

Example path in script:

```
C:/PathToBed.bed
```

---

### 2. FoxM1 ChIP-seq peaks

BED file containing FoxM1 peak coordinates.

Example path in script:

```
C:/PathToBed.bed
```

---

### 3. ReMap transcription factor catalog

Genome-wide transcription factor binding dataset used for enrichment analysis.

Example path:

```
C:/PathToRemapCatalog.bed
```

The catalog should correspond to the **same genome build** as the ChIP-seq peaks (e.g., hg38).

---

# Pipeline Steps

## 1. Load BED files

The script converts BED files into genomic ranges using:

```
bedToGranges()
```

This allows efficient genomic interval operations.

---

## 2. Identify overlapping peaks

Peaks from both datasets are expanded by **±5 kb** to allow detection of nearby regulatory interactions.

Overlaps are detected using:

```
findOverlaps()
```

If no overlaps are found, the script stops with an error.

The overlapping peaks are saved as:

```
PathToNewOverlapFile.bed
```

---

## 3. Create overlap loci table

For each overlapping region the script records:

* chromosome
* start position
* end position
* locus identifier
* p53 peak score
* FoxM1 peak score

Duplicate loci are removed to avoid repeated entries when multiple FoxM1 peaks overlap the same p53 region.

Output file:

```
PathToTable.txt
```

---

## 4. Assign nearest genes

The script assigns the **closest gene** to each overlap locus using Ensembl gene annotations retrieved via `biomaRt`.

Steps performed:

1. Download gene coordinates from Ensembl
2. Compute the center of each locus
3. Calculate the distance between the locus center and gene centers
4. Assign the nearest gene symbol

Output file:

```
PathToLoci.txt
```

Columns in the final table:

| Column      | Description                   |
| ----------- | ----------------------------- |
| locus       | genomic coordinate identifier |
| chr         | chromosome                    |
| start       | start coordinate              |
| end         | end coordinate                |
| gene_symbol | nearest gene                  |

The table is sorted alphabetically by gene symbol.

---

## 5. Prepare regions for enrichment

Overlapping regions are expanded by **±1 kb** before enrichment analysis to capture nearby transcription factor binding events.

---

## 6. Transcription factor enrichment analysis

Enrichment is performed using the **ReMap TF binding catalog**.

Function used:

```
enrichment()
```

This identifies transcription factors whose binding sites significantly overlap the p53–FoxM1 regions.

Output file:

```
PathToEnrichment.txt
```

Key output columns include:

| Column      | Description              |
| ----------- | ------------------------ |
| category    | TF dataset identifier    |
| nb.overlaps | number of overlaps       |
| effect.size | enrichment strength      |
| p.value     | statistical significance |

---

# Visualization

## Dot Plot – Top 20 enriched TFs

The dot plot displays the **top 20 enriched transcription factors** ranked by effect size.

Visual encoding:

* X-axis: effect size (log enrichment)
* Y-axis: transcription factor
* dot size: number of overlapping peaks
* dot color: −log10(p-value)

---

## Bar Plot – TF overlap counts

A horizontal bar plot displays the **top 20 transcription factors ranked by number of overlapping peaks**.

Axes:

* X-axis: transcription factor
* Y-axis: number of overlapping peaks

---

# Output Files

| File                     | Description                             |
| ------------------------ | --------------------------------------- |
| PathToNewOverlapFile.bed | overlapping genomic regions             |
| PathToTable.txt          | overlap loci with p53 and FoxM1 scores  |
| PathToLoci.txt           | overlap loci with nearest genes         |
| PathToEnrichment.txt     | transcription factor enrichment results |

---

# Notes

* All input files should use the **same genome assembly** (e.g., hg38).
* Large ReMap catalogs may require significant memory.
* Ensembl queries require an active internet connection.

---

# Possible Extensions

This workflow can be extended with:

* motif enrichment analysis (HOMER, MEME)
* pathway enrichment of identified genes
* visualization in genome browsers (IGV, UCSC)
* distance-to-TSS analysis
* comparison across multiple cell lines

---

# Author

Analysis pipeline for studying **co-binding of p53 and FoxM1 transcription factors** and identifying additional regulatory factors enriched in shared genomic regions.
=======
# ChIPseq_overlap
This script requires the query of 2 ChIPseq bed files. It overlaps them, and maps their overlapping loci to genes in the Hg38 genome.
>>>>>>> aaeda1b35610967ad4d383eac16b427786ccaaae
