# BEREN
A comprehensive bioinformatic workflow for giant virus and Preplasmiviricota retrieval from metagenomes.
<img src="https://github.com/BenMinch/BEREN/blob/main/Beren_logo.png" width=70% height=70%>
## What BEREN does
BEREN is a comprehensive pipeline for accessing the diversity and functional potential of giant viruses (NCLDV) and Preplasmiviricota viruses (Polinton-like viruses, Virophages, Polintons) in metagenomes. It can be used to get overall diversity (marker genes), longer contigs, and genomes of these eukaryotic viruses and predict their functional potential and taxonomy. BEREN is modular so you can choose which modules to run on a given sample. 
<img src="https://github.com/BenMinch/BEREN/blob/main/BEREN_pipeline.png" width=100% height=100%>
# Citation
Coming soon.
# Installation
### 1. Cloning this repository
This repository has almost everything you need to get started, including all the scripts and resources BEREN uses. Simply run `git clone https://github.com/BenMinch/BEREN` to clone the repository into your current folder.
### 2. Setting up the conda environment
A yml file has been provided to aid in setting up the conda environment. Simply run `conda env create -f BEREN_environment.yml` to set up your conda environment and `conda activate BEREN` to activate it.
# Quick Start
### Running all modules on the example files
We have provided a test metagenome with a sample coverage file to ensure BEREN works properly. To run BEREN you must open a terminal window within the folder where the `BEREN.py` script is along with the `hmm`, `scripts`, and `resources` folders. Here is how to run the example files with all modules `python BEREN.py -i Examples/example.fa -o Test -m all -cov Examples/example.coverage -t 6`.

Running this should output a folder called `Test` that should have around 313 markers, 2 high-quality genomes, 5 partial genomes, 20 NCLDV contigs, and 0 Preplasmiviricota viruses.

### A description of parameters and flags
1. `-i`: This is the input metagenome assembly. (required)
2. `-cov`: This is the coverage file in metabat2 format. This is only required for metagenomic binning in the NCLDV_bins module. This file can be generated using coverm.
3. `-o`: Output folder name (required)
4. `-t`: Threads (I usually run with 6-10 for optimal speed)
5. `-m`: Modules to run. The default is `all` but you can choose between `NCLDV_contigs`, `NCLDV_bins`, `NCLDV_marker`, `metabolism`, and `Virophage`. You can choose multiple modules by separating them with a comma.
6. `-db`: The database that you want to use for protein annotations. The default is `gvog` but you can also choose between `Pfam` and `KEGG`. You can choose multiple by separating them with a comma. (Only required for metabolism module)
7. `-part`: A flag for whether or not to include partial genomes in metabolism analysis. The default is false.

# Modules
All main outputs are found in the `Final_Results` folder that is created inside your output directory. If you want to see intermediate files or MAGs that aren't giant viruses, you can look into individual module folders.
## NCLDV_marker
<p>The NCLDV_marker module gives you the total diversity of giant viruses within your metagenome through the assessment of NCLDV marker genes (hallmark genes used for phylogenetic analysis). This module will identify these genes (Major capsid protein, DNA PolB, SFII, TFIIB, Topo2, RNAPS, RNAPL, A32, and VLTF3) and create a phylogeny for PolB using reference sequences for all NCLDV families and orders</p>

<p>In addition, BEREN also searches for marker genes (MCPs) of newly discovered giant viruses such as Mirusviruses, Egovirales, and Mriyaviruses </p>

**Outputs** <br>
`NCLDV_Markers.faa`: Protein file with all found marker genes. They are labeled "GVOGm". Refer to https://github.com/faylward/ncldv_markersearch for info on this naming scheme.<br>
`NCLDV_Marker_PolB_Tree.nwk`: A treefile of NCLDV PolB proteins in newick format to be displayed on itol or elsewhere. The annotation file can be found in the resources folder. It is called `Tree_Labels.csv`.<br>
`EGO_Mirus_Mriya_Markers.faa`: All of the markers for new viruses (MCPs). 
## NCLDV_contigs
This module gives you all the NCLDV contigs in your metagenome. You can treat these as "ASVs" or "OTUs" to represent distinct NCLDV populations. By default, only contigs > 5000 kbp are considered. These contigs are identified using ViralRecall. 

**Outputs**<br>
`NCLDV_Contigs.fasta`: A single fasta file with all NCLDV contigs within it. Note, if you do this and bin module some of these contigs will likely overlap.

## NCLDV_bins
<p> The NCLDV_bins module can be used to recover high-quality and partial giant virus metagenome-assembled genomes (GVMAGs). GVMAGs are considered high quality if they contain a Capsid protein and 3 other marker genes. Partial genomes are those that contain at least one NCLDV marker gene and have a positive ViralRecall score. Both high-quality and partial GVMAGs are cleaned through a screening process to remove misbinned contigs or those that aren't of viral origin. </p>
<p> After GVMAGs are recovered, taxonomic information will be predicted using TIGTOG (https://github.com/anhd-ha/TIGTOG)</p>

**If you have Mirusviricota or Mriyavirus MAGs in your sample, they will be found among the partial bins.** <br>

**Outputs**<br>
`clean_genomes`: A folder containing all high-quality GVMAGs. <br>
`clean_partial_genomes`: A folder containing all partial GVMAGs <br>
`Good_Bins_TIGTOG.prediction_result.tsv`: A file containing TIGTOG taxonomy of high-quality GVMAGs. <br>
`Partial_Bins_TIGTOG.prediction_result.tsv`: A file containing TIGTOG taxonomy of partial GVMAGs. <br>
`NCLDV_Genome_Summary.tsv`: A file with information on genome marker gene content, ViralRecall score, and decontamination stats. <br>

## Metabolism
<p> The metabolism module gives the user information about the proteins and functional potential stored within their GVMAGs and NCLDV contigs within their sample. This module runs protein prediction and annotation on the GVMAGs to find key metabolic genes. The user has the choice of which databases to use for protein annotation, but the default is the GVOG (giant virus orthologous genes) database. Pfam and KEGG are also available, but an additional download is required for KEGG. </p>

**A note on downloading KEGG**<br>
The KEGG database can be downloaded using the following command `wget https://www.genome.jp/ftp/db/kofam/profiles.tar.gz` and then extracting the HMM profile, renaming it to `KEGG.hmm` and placing it in the `hmm` folder.<br>

**Outputs**<br>
`All_bin_proteins_final.csv`: This contains all protein annotations for bins (high quality and partial if you use `-part` flag)<br>
`All_contig_proteins_final.csv`: Protein annotations for NCLDV contigs.<br>
`NCLDV_bins_metabolic_genes.csv`: Metabolic gene annotations of interest for bins.<br>
`NCLDV_bins_AMG_bubble_plot.pdf`: A bubble plot of relevant metabolic genes in GVMAGs.<br>

## Virophage
The virophage module identifies Preplasmiviricota (virophages and Polinton-like viruses) genomes inside of your metagenome, classifies them as either virophage or PLV, and annotates the recovered genomes. Protein annotation is done using Pfam and a custom hmm database of protein clusters derived from [Bellas et al.](https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-020-00956-0)

**Outputs**<br>
`PLV_Good_Regions`: All the individual Preplasmiviricota genomes recovered.<br>
`PLV_Good_Proteins`: All the predicted proteins for genomes. <br>
`06_Virophage_final_affiliation.tsv`: Prediction of whether genome is virophage or PLV.<br>
`PLV_VP_contigs.csv`: A summary file of marker genes found in each genome. <br>
`Virophage_proteins_final.csv`: Annotations for all genome proteins.

# Batch Mode
A batch script has been provided to run BEREN on many assemblies simultaneously. The only input for this program is a reference file that has full paths to each sample, an output folder, the modes you want to run for each sample (separated by a, if specifying multiple), and the coverage file absolute path if running NCLDV_bins module. An example of this file has been provided in the examples folder.
# Programs used in the Pipeline
BEREN is built on the shoulders of many other great bioinformatic tools. Check out their GitHub pages to see if you want to use any part of the pipeline separately or want to know the default parameters.<br>
1. [NCLDV_Markersearch](https://github.com/faylward/ncldv_markersearch)
2. [NuPhylo](https://github.com/BenMinch/NuPhylo)
3. [ViralRecall](https://github.com/faylward/viralrecall)
4. [Hmmer](https://github.com/EddyRivasLab/hmmer)
5. [AnnoMazing](https://github.com/BenMinch/AnnoMazing)
6. [TIGTOG](https://github.com/anhd-ha/TIGTOG)
7. [Virophage_affiliation](https://github.com/simroux/ICTV_VirophageSG/tree/main)
8. [Metabat2](https://bitbucket.org/berkeleylab/metabat/src/master/)
9. [Prodigal-GV](https://github.com/apcamargo/prodigal-gv)

# Copyright
BEREN Copyright (C) 2024 Benjamin Minch

This program is free software: you can redistribute it and/or modify it under the terms of the MIT License.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the MIT License for more details.
