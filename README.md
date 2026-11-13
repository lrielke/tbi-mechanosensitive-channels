# Mechanosensitive Ion Channels in the Ageing Human Brain after Traumatic Brain Injury

An exploratory bioinformatics analysis of mechanosensitive ion channel gene expression 
in post-mortem human brain tissue from older adults with and without a history of 
traumatic brain injury (TBI).

## Background

Traumatic brain injury is fundamentally a mechanical event — forces stretch and shear 
brain tissue, activating mechanosensitive ion channels that convert physical force into 
cellular signals. Channels such as PIEZO1, PIEZO2, TRPV4, and the TREK/TRAAK potassium 
channels (KCNK2, KCNK4, KCNK10) are expressed across multiple brain cell types, yet 
their role in the long-term consequences of TBI — including neurodegeneration and 
inflammation — remains poorly understood. TBI is a known risk factor for dementia, 
making this question directly relevant to Alzheimer's disease research.

## Dataset

This project uses the Allen Institute **Aging, Dementia and TBI Study** 
(aging.brain-map.org), a publicly available dataset of RNA-seq gene expression from 
107 post-mortem donors (54 no-TBI, 53 TBI history with loss of consciousness) from the 
Adult Changes in Thought (ACT) cohort. The dataset includes 377 samples from four brain 
regions: hippocampus, temporal cortex, parietal cortex, and frontal white matter, 
alongside donor metadata, neuropathology quantification, and Luminex protein measurements.

## Research Questions

1. Is expression of mechanosensitive channel genes different between donors with and 
   without a history of TBI?
2. Does expression differ across brain regions, particularly white matter?
3. Does channel expression correlate with inflammatory proteins, tau, or amyloid pathology?
4. Which cell types are these channels associated with, based on marker gene scores?
5. Does dementia status independently associate with channel expression?

## Methods

- **Expression analysis:** normalised FPKM values (log₂-transformed) for 10 target genes
- **Statistical modelling:** linear mixed effects models 
  (`expression ~ TBI + dementia + age + sex + (1|donor)`) to account for multiple 
  samples per donor, implemented in statsmodels (Python)
- **Multiple comparisons:** Benjamini-Hochberg FDR correction across all tests
- **Correlations:** Spearman rank correlations with pathology and protein measures
- **Cell-type scoring:** average expression of published marker genes 
  (Zhang et al. 2016) as a proxy for cell-type composition in bulk tissue

## Key Findings

- All 10 mechanosensitive channel genes are expressed in aged human brain tissue
- **TMEM63A and PIEZO2 show the strongest TBI associations in white matter** 
  (FWM: coef=+0.44 and +0.33 respectively, p<0.05 uncorrected) — no results survive 
  FDR correction, consistent with the small sample size and long post-injury interval
- **Regional differences are more striking than TBI differences** — PIEZO2 and TRPV1 
  are substantially enriched in white matter compared to grey matter regions
- **TMEM63A expression strongly tracks oligodendrocyte markers** (ρ=0.95) and PIEZO2 
  similarly (ρ=0.86), suggesting these channels are primarily expressed in 
  oligodendrocytes — the cell type most vulnerable to TBI white matter injury
- **Channel expression correlates with GFAP** (astrocyte marker; TMEM63A ρ=0.46, 
  PIEZO2 ρ=0.42) and inversely with amyloid pathology (TMEM63A ρ=−0.29), linking 
  mechanosensitive channels to both astrocyte biology and neurodegeneration

## Limitations

- Small sample (107 donors) — insufficient power to survive FDR correction at 
  region-specific level
- TBI history is self-reported and injuries occurred years to decades before death — 
  this reflects long-term changes, not acute responses
- Bulk tissue RNA-seq mixes cell types — cell-type scores are estimates only, 
  not true deconvolution
- RNA expression does not equal channel protein levels or function

## Repository Structure
notebooks/
01_download_and_qc.ipynb — data loading, cohort description, QC
02_channel_expression.ipynb — expression visualisation by TBI and region
03_mixed_models.ipynb — mixed effects models, region-specific analysis
04_correlations_proteins_pathology.ipynb — correlations with pathology and inflammation
05_cell_type_scores.ipynb — cell-type marker scoring and channel association
figures/ — all output figures
src/ — reusable functions (in development)
environment.yml — conda environment for reproducibility


## How to Reproduce

1. Clone this repository
2. Create the conda environment: `conda env create -f environment.yml`
3. Activate: `conda activate tbi-channels`
4. Download data from [aging.brain-map.org/download/index](https://aging.brain-map.org/download/index) 
   and place CSV files in `data/`
5. Run notebooks in order (01 → 05) using JupyterLab

## References

- Allen Institute for Brain Science. Aging, Dementia and TBI Study. aging.brain-map.org
- Miller JA et al. (2017). Neuropathological and transcriptomic characteristics of the 
  aged brain. *eLife* 6:e31588
- Zhang Y et al. (2016). Purification and characterization of progenitor and mature 
  human astrocytes reveals transcriptional and functional differences with mouse. 
  *Neuron* 89(1):37-53
