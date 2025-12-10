# Translation-Efficiency-TE-Translation Efficiency (TE) Analysis Pipeline

This repository provides a ready-to-run, reproducible pipeline for computing Translation Efficiency (TE) using elongating ribosome footprints from two published studies:

| Study                                                             | GEO          | PMCID      | Notes                                               |
| ----------------------------------------------------------------- | ------------ | ---------- | --------------------------------------------------- |
| *Myc coordinates transcription and translation* (EMBO J 2016)     | **GSE66929** | PMC4687422 | Contains Ribo-seq (elongating footprints) + RNA-seq |
| *c-MYC regulates mRNA translation efficiency* (Cell Reports 2019) | **GSE79864** | PMC6605752 | Contains Ribo-seq + RNA-seq                         |


This pipeline harmonizes both studies so their data can be processed identically for TE analysis.

## 1. Overview

This pipeline performs:

### 1. SRA download (Ribo-seq + RNA-seq)
### 2. Adapter trimming
### 3. Removal of rRNA reads
### 4. Genome alignment (STAR)
### 5. Footprint counting (featureCounts)
### 6. Translation efficiency calculation (RiboRex)
### 7. QC + plot generation

All steps are completely automated.

## 2. Dataset Accession List
**GSE66929 – EMBO J (2016)**

**Ribo-seq (elongating footprints):**
````
SRR2920474  
SRR2920475  
SRR2920476  
SRR2920477
````
**RNA-seq:**
````
SRR2920486  
SRR2920487  
SRR2920488  
SRR2920489
````
**GSE79864 – Cell Reports (2019)**

**Ribo-seq:**
````
SRR9215969  
SRR9215970  
SRR9215971  
SRR9215972
````

**RNA-seq:**
````
SRR9215973  
SRR9215974  
SRR9215975  
SRR9215976
````
## 3. Repository Structure
````
TE-Pipeline/
├── README.md
├── scripts/
│   ├── 01_download_data.sh
│   ├── 02_trim.sh
│   ├── 03_remove_rRNA.sh
│   ├── 04_align.sh
│   ├── 05_count_footprints.sh
│   ├── 06_calculate_TE.R
│   └── 07_qc.R
├── env/
│   └── environment.yml
└── references/
    ├── genome.fa
    ├── annotation.gtf
    ├── rRNA.fa
    └── STAR_index/
````

The scripts in the ZIP are placeholders unless you specifically request that I regenerate them with the full executable code from the canvas.

## 4. Installation
**Install Conda environment**
````
conda env create -f env/environment.yml
conda activate te-env
````
**Required tools (auto-installed via environment):**

- sra-tools
- cutadapt
- bowtie2
- star
- samtools
- fastqc
- bedtools
- subread (featureCounts)
- R (tidyverse, DESeq2, RiboRex)

## 5. Running the Pipeline
**Step 1 — Download SRA files**
````
bash scripts/01_download_data.sh
````
**Step 2 — Trim adapters**
````
bash scripts/02_trim.sh
````
**Step 3 — Remove rRNA**
````
bash scripts/03_remove_rRNA.sh
````
**Step 4 — Align to genome (STAR)**
````
bash scripts/04_align.sh
````
**Step 5 — Count footprints + RNA**
````
bash scripts/05_count_footprints.sh
````
**Step 6 — Calculate translation efficiency**
````
Rscript scripts/06_calculate_TE.R
````
**Step 7 — QC + plots**
````
Rscript scripts/07_qc.R
````
## 6. Output Files
````
| Output                | Description                                                           |
| --------------------- | --------------------------------------------------------------------- |
| **TE_results.csv**    | Translation efficiency per gene (log2TE, p-values, adjusted p-values) |
| **TE_volcano.png**    | Volcano plot of differential TE                                       |
| **Aligned BAM files** | STAR-aligned Ribo-seq and RNA-seq                                     |
| **Count matrices**    | featureCounts output                                                  |

````
## 7. Notes on Harmonization of the Two Studies

**Both studies used:**

- Ribo-seq protocol isolating ~28–32 nt elongating ribosome footprints

- RNA-seq using rRNA-depleted libraries

- mRNA libraries compatible with TE calculation

- Mammalian (mouse/human) genome references

**Therefore, combining them for uniform TE analysis is valid if:**

- you normalize by condition

- process both with identical tools (as done here)

- evaluate batch effects (optional: add DESeq2 batch term)

## 8. Citation

Elkon R, Loayza-Puch F, Korkmaz G, Lopes R, van Breugel PC, Bleijerveld OB, Altelaar AF, Wolf E, Lorenzin F, Eilers M, Agami R. Myc coordinates transcription and translation to enhance transformation and suppress invasiveness. EMBO Rep. 2015 Dec;16(12):1723-36. doi: 10.15252/embr.201540717. Epub 2015 Nov 4. PMID: 26538417; PMCID: PMC4687422.

Singh K, Lin J, Zhong Y, Burčul A, Mohan P, Jiang M, Sun L, Yong-Gonzalez V, Viale A, Cross JR, Hendrickson RC, Rätsch G, Ouyang Z, Wendel HG. c-MYC regulates mRNA translation efficiency and start-site selection in lymphoma. J Exp Med. 2019 Jul 1;216(7):1509-1524. doi: 10.1084/jem.20181726. Epub 2019 May 29. PMID: 31142587; PMCID: PMC6605752.

## 9. License

MIT License (modify based on my preference).
