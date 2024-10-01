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
We have provided a test metagenome with a sample coverage file to ensure BEREN works properly. To run BEREN you must open a terminal window within the folder where the `BEREN.py` script is along with the `hmm`, `scripts`, and `resources` folders. Here is how to run the example files with all modules `python BEREN.py -i Examples/example.fa -o Test -m all -cov Examples/example.coverage -t 6`. 

Running this should output a folder called `Test` that should have 

### A description of parameters and flags
1. `-i`: This is the input metagenome assembly. (required)
2. `-cov`: This is the coverage file in metabat2 format. This is only required for metagenomic binning in the NCLDV_bins module. This file can be generated using coverm.
3. `-o`: Output folder name (required)
4. `-t`: Threads (I usually run with 6-10 for optimal speed)
5. `-m`: Modules to run. The default is `all` but you can choose between `NCLDV_contigs`, `NCLDV_bins`, `NCLDV_marker`, `metabolism`, and `Virophage`. You can choose multiple modules by separating them with a comma.
6. `-db`: The database that you want to use for protein annotations. The default is `gvog` but you can also choose between `Pfam` and `KEGG`. You can choose multiple by separating them with a comma. (Only required for metabolism module)
7. `-part`: A flag for whether or not to include partial genomes in metabolism analysis. The default is false.

# Modules
### NCLDV_marker
<p>The NCLDV_marker module gives you the total diversity of giant viruses within your metagenome through the assessment of NCLDV marker genes (hallmark genes used for phylogenetic analysis). This module will identify these genes (Major capsid protein, DNA PolB, SFII, TFIIB, Topo2, RNAPS, RNAPL, A32, and VLTF3) and create a phylogeny for PolB using reference sequences for all NCLDV families and orders</p>

<p>In addition, BEREN also searches for marker genes (MCPs) of newly discovered giant viruses such as Mirusviruses, Egovirales, and Mriyaviruses </p>

<p> **Outputs** <br>
`NCLDV_Markers.faa`: Protein file with all found marker genes. They are labeled "GVOGm". Refer to https://github.com/faylward/ncldv_markersearch 
