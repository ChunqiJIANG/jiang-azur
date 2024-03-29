# This is the list of created environments and installed main packages in 'jiang@azur'


#example
> **Environment 'name':**
- package_01-version
  - dependent package_001-version
  - dependent package_002-version
- package_02-version
- package_03-version
- ...


## 2023-03-01
> **Environment-00 'base':** (*default*)
- mamba-1.0.0
- conda-22.11.1
- python-3.9.12
- screen-4.8.0


> **Environment-01 'anvio-7.1':** (*conda activate anvio-7.1*)
- anvio-7.1
  - python-3.6.15
  - sqlite-3.40.0 
  - prodigal-2.6.3
  - mcl-14.137
  - muscle-3.8.1551
  - hmmer-3.3.2
  - diamond-2.0.15
  - blast-2.5.0
  - megahit-1.2.9
  - spades-3.15.5
  - bowtie2-2.3.5
  - bwa-0.7.17
  - samtools-1.16.1
  - fasttree-2.1.11
  - fastani-1.33


> **Environment-02 'assembly':** (*conda activate assembly*)
- unicycler-0.4.8 
  - pilon-1.24
  - racon-1.5.0
  - spades-3.15.5
  - minimap2-2.24
  - bowtie2-2.5.0
- medaka-1.7.2 
  - python-3.8.15
- flye-2.9.1
- filtlong-0.2.1
- fastp-0.223.2
- seqkit-2.3.1 
- fastqc-0.11.9


> **Environment-03 'gtdbtk-2.1.1':** (*conda activate gtdbtk-2.1.1*)
- gtdbtk-2.1.1
  - numpy-1.23.5
  - prodigal-2.6.3
	- fasttree-2.1.11
	- fastani-1.3


> **Environment-04 'checkm':** (*conda activate checkm*)
- checkm-genome-1.2.2
	- matplotlib-base-3.6.3
	- numpy-1.24.2
	- pysam-0.20.0
 - **gunc-1.0.5** (2023-07-06 add)
	- pandas-2.0.3 (error => 1.5.1)
	- diamond-2.0.4


> **Environment-05 'pydata':** (*conda activate pydata*)
- ipython-8.9.0 
- numpy-1.24.1
- pandas-1.5.3
- matplotlib-3.6.3 
- scikit-learn-1.2.1
- seaborn-0.12.2 
- jupyter-1.0.0
- notebook-6.5.2


> **Environment-06 'r-env':** (*conda activate r-env*)
- r-base-4.2.2
- r-essentials-4.2
- jupyter-1.0.0
- notebook-6.5.2
- ipykernel-6.21.1
- r-irkernel-1.3.2
- WGCNA 
  

> **Environment-07 'unicycler050':** (*conda activate unicycler050*)
- unicycler-0.5.0
	- racon-1.5.0
	- spades-3.15.5
- pilon-1.24
- canu-2.2

## 2023-03-10
> **Environment-08 'qiime2-2023.2':** (*conda activate qiime2-2023.2*)
- QIIME 2 Core 2023.2 distribution
	- q2-alignment
	- q2-composition
	- q2-cutadapt
	- q2-dada2
	- q2-deblur
	- q2-demux
	- q2-diversity
	- q2-diversity-lib
	- q2-emperor
	- q2-feature-classifier
	- q2-feature-table
	- q2-fragment-insertion
	- q2-gneiss
	- q2-longitudinal
	- q2-metadata
	- q2-phylogeny
	- q2-quality-control
	- q2-quality-filter
	- q2-sample-classifier
	- q2-taxa
	- q2-types
	- q2-vsearch
- q2-SCNIC
	- blas-1.1

## 2023-03-28
> **Environment-09 'ncbi':** (*conda activate ncbi*)
- ncbi-blast-2.13.0+
- **diamond-2.1.8** (2023-07-12)

## 2023-07-03
> **Environment-10 'prokka':** (*conda activate prokka*)
- prokka-1.14.6
	- blast-2.14.0
 	- prodigal-2.6.3


***
> **Environment-xx '':** (*conda activate *)
