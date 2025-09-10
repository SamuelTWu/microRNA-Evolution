# miRNA Repertoire Evolution  

This repository contains the code and analysis pipelines used in our study on the evolution and organization of microRNA (miRNA) repertoires across species.  

## Overview  

MicroRNAs (miRNAs) are key post-transcriptional regulators, but the number and diversity of miRNA families vary widely across species.  
In this project, we show that this variability is not random, but reflects the optimization of gene regulatory network efficiency.  

Key findings:  
- Vertebrate miRNA repertoires are **selectively organized** to maximize transcriptome targeting while minimizing redundancy and genetic risk.  
- Invertebrate repertoires display **random seed usage patterns**.  
- miRNA **number correlates with diversity—not quantity—of 3′UTRs**.  
- Targeting efficiency emerges as an **intensive property** of seed sequence composition.  

These findings suggest that miRNA repertoires evolved not simply to expand regulation, but to ensure **robustness, adaptability, and efficient network performance**. 

# Methods  

This repository implements the computational and analytical methods described in the study.  
All pipelines are reproducible using the provided scripts and notebooks.  

---

## Identifying miRNA Families  

- Mature miRNA sequences were obtained from **mirGeneDB v3.0**.  
- The **canonical 7-nt seed region (positions 2–8)** was extracted for each miRNA.  
- Species-specific seeds were identified by comparing each species’ repertoire to all others.  
- Total seed diversity per species was quantified to assess repertoire composition.  
- Species-level annotations were retrieved from mirGeneDB.  

---

## 7-mer Sequence Space Heatmap  

A 2D heatmap was generated to visualize the compositional landscape of all possible 7-mer nucleotide sequences:  

- 128 × 128 matrix, with unique 7-mers positioned according to nucleotide frequency.  
- Quadrant-level clustering preserved local similarity (e.g., "AAAAAAA", "TTTTTTT").  
- Color palette:  
  - A = Green  
  - U = Red  
  - C = Blue  
  - G = Yellow  
- Color intensity reflects the mean RGB vectors of nucleotide composition.  
- Final matrix was saved as **CSV** for reproducibility and downstream analysis.  

This visualization enables intuitive interpretation of nucleotide bias across seed space and serves as a reference for motif analyses.  

---

## Transcriptome Seed Match Quantification  

- Transcript sequences for **130 metazoan species** were retrieved from **Ensembl Biomart (release 109)**.  
- Preprocessing included FASTA parsing, removal of placeholders, and format standardization.  
- Seed match frequencies were computed using a **sliding-window algorithm**.  
- Frequencies were:  
  - Compiled into matrices  
  - **Z-normalized** for cross-species comparability  
  - Transformed using a **sigmoid function** for contrast  

A full pipeline parses miRNA seeds, generates reverse complements, and computes utilization as the fraction of used 7-mers out of all 16,384 possible combinations.  

---

## Minimum Free Energy (MFE) Calculations  

- Assessed thermodynamic stability of seed pairing in the zebrafish genome.  
- All 16,384 possible 7-mers were evaluated using **ViennaRNA RNAcofold**.  
- Gibbs free energy (ΔG) was estimated as a proxy for duplex stability.  
- Visualization reflects the global thermodynamic landscape of canonical 7-mers.  

---

## G-Quadruplex Prediction  

- Investigated structural determinants underlying underutilized seeds.  
- **G4NN scores** were computed using **G4RNA Screener**.  
- Scores were reshaped into a 2D matrix to visualize “G-Quadruplex G-Probability”.  
- Enabled assessment of whether G-rich motifs overlap with seed underutilization zones.  

---

## Quantifying miRNA Targets  

- Canonical seed sites (7mer-A1, 7mer-m8, 8mer) were scanned across 3′UTRs.  
- Data sources:  
  - Mature miRNAs from **mirGeneDB**  
  - 3′UTRs from **Ensembl Biomart**  
- Proportion of genes regulated by miRNAs was computed per species.  
- Comparative scatter plots:  
  - X-axis = number of unique 7-mer seeds  
  - Y-axis = proportion of 3′UTRs targeted  
  - Color-coded by phylogenetic classification (mammals, non-mammalian vertebrates, invertebrates)  

---

## 3′UTR Analysis  

- Counted annotated transcripts per species.  
- Measured mean 3′UTR length.  
- Compared duplicated vs. non-duplicated fish genomes.  
- Plotted distributions of:  
  - Total 3′UTR counts  
  - Mean length  
  - Number of distinct miRNA seeds  

---

## Random vs. Optimal Simulations  

To test whether observed repertoires reflect optimization:  

- **Real networks**: empirical miRNA-target mappings  
- **Random networks**: uniformly shuffled target assignments  
- **Optimal networks**: greedy algorithm maximizing coverage with minimal seeds  

Coverage was computed as the fraction of transcripts regulated across increasing numbers of miRNA families.  

---

## Seed Rank Analysis  

- Ranked seeds by transcriptome coverage.  
- Compared **real**, **random**, and **optimal** seed sets (30 replicates).  
- Performed cross-species experiments:  
  - Human-only seeds tested on zebrafish genome, and vice versa.  
  - Measured coverage deviation relative to optimal and random seeds.  

---

## Octopus Genome Analysis  

- 3′UTRs from *Octopus bimaculoides* (Ensembl Metazoa).  
- miRNA sequences from **mirGeneDB v3.0**.  
- Data used in Figures 3 and 4.  

---

## N-Target Prediction  

- Simulated coverage of human genes using random seed sets (100–1600 seeds).  
- For each size:  
  - 30 replicates  
  - Coverage measured at thresholds of ≥1, 2, 3, or 4 seed hits per gene  
- Compared observed biological seed sets to baseline random expectations.  

---

## Nucleotide Composition Bias  

- Performed **Chi-squared goodness-of-fit tests** comparing:  
  - Observed nucleotide distributions of real seeds  
  - Expected distributions of all possible 7-mers  
- Tested for significant deviations in nucleotide usage.  

---

## Mammals vs. Non-Mammals  

- Proportion of genes covered by canonical seeds compared between mammals and non-mammals.  
- Statistical test: **two-sided Wilcoxon rank-sum**.  

---

## Seed Rank Distribution Similarity  

- Computed **Wasserstein distances** between real, random, and optimal coverage distributions.  
- Used **permutation tests (10,000 iterations)** to assess statistical significance.  

---

## Correlation Between G-Quadruplexes and Seeds  

- Each 7-mer was assigned a G4-probability score.  
- Compared against binary seed presence using **Pearson correlation**.  

---

## Notes on AI Assistance  

During manuscript preparation, the authors used ChatGPT for language refinement.  
All content was reviewed and edited by the authors, who take full responsibility for accuracy.  

---
