# BEREN
A comprehensive bioinformatic workflow for giant virus and Preplasmiviricota retrieval from metagenomes
<img src="https://github.com/BenMinch/BEREN/blob/main/Beren_logo.png" width=70% height=70%>
## What BEREN does
BEREN is a comprehensive pipeline for accessing the diversity and functional potential of giant viruses (NCLDV) and Preplasmiviricota viruses (Polinton-like viruses, Virophages, Polintons) in metagenomes. It can be used to get overall diversity (marker genes), longer contigs, and genomes of these eukaryotic viruses and predict their functional potential and taxonomy. BEREN is modular so you can choose which modules to run on a given sample. 
<img src="https://github.com/BenMinch/BEREN/blob/main/BEREN_pipeline.png" width=100% height=100%>
# Installation
### 1. Cloning this repository
This repository has almost everything you need to get started, including all the scripts and resources BEREN uses. Simply run `git clone https://github.com/BenMinch/BEREN` to clone the repository into your current folder.
### 2. Setting up the conda environment
A yml file has been provided to aid in setting up the conda environment. Simply run `conda env create -f BEREN_environment.yml` to set up your conda environment and `conda activate BEREN` to activate it.
# Quick Start
### Running all modules on the example files
We have provided a test metagenome with a sample coverage file to ensure BEREN works properly. To run BEREN you must open a terminal window within the folder where the `BEREN.py` script is along with the `hmm` , `scripts`, and `resources` folders. 
