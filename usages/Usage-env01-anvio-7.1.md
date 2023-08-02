
This page introduces the usages in the environment of **'anvio-7.1'**.

<p align="right"> Updated on 2023-08-02 </p>

#### Usage description
Usage text 
```
  # annotation
  (environment) ~/location$ command line
```


### Move to your workplace and activate the environment first
```
  # don't forget to change to your directory first
  (base) jiang@azur:~$ cd user_name
  
  # check the current enviroment at the beginning of the command line, like the 'base' here
  (base) jiang@azur:~/user_name$
  # if not the target environment, activate it by 'conda activate'
  (base) jiang@azur:~/user_name$ conda activate anvio-7.1
  # check again at the beginning, now it is 'anvio-7.1', good to go
  (anvio-7.1) jiang@azur:~/user_name$ 
```


## Let's start !!!

Here I mainly introduce the three usages:
- Pangenomic
- Metagenomics
- Meta-pan genomics

### Usage01: [Pangenomics]()

#### 01-00: EASY WAY - Using jiang's script
One script for easy use of pangenome database preparation was written.
> USAGE: xxx/00-scripts/script_CJ_pangenomics_workflow_full_Ver6.sh GENOME_DIR RUN_NAME NUM_THREADS
>> This is the workflow for Pangenomic analyses, written by Chunqi Jiang. If you have any questions, please let him know.

Before start:
1. put all your genomes (.fna) in a directory, such as '00-GENOMES'
	*Please limit the name characters to ASCII letters, digits, and the underscore*
```
# check everything is ready
(anvio-7.1) jiang@azur:~/user_name/your_place$ ls
   00-GENOMES 
# start script
(anvio-7.1) jiang@azur:~/user_name/your_place$ ~/user_jiang/00-scripts/script_CJ_pangenomics_workflow_full_Ver6.sh GENOME_DIR RUN_NAME NUM_THREADS
	# example
	(anvio-7.1) jiang@azur:~/user_name/your_place$ ~/user_jiang/00-scripts/script_CJ_pangenomics_workflow_full_Ver6.sh 00-GENOMES Test_Jiang 20

That's it! Easy right?
``` 
>*GENOME_DIR: the directory containing all your genome files;*

>*RUN_NAME: your project name, anything is ok;*

>*NUM_THREADS: number of threads you want to use, usually recommend 20 here.*


#### 01-01: HARD BUT WORTH - Step by Step

```
Checking your input FASTA files
## Re-formatting your input FASTA (simplify the header lines of FASTA files for genomes)
# one cmd for all (multiple files)
(anvio-7.1) jiang@azur:~/user_name/your_place$ for f in *.fasta; do anvi-script-reformat-fasta $f -o ../01_SIMPLIFY/${f%.*}_simplify.fasta -l 0 --simplify-names --seq-type NT ; done

# one by one (one file per time) if you like


```

#### 01-02 Converting FASTA files into anvi’o contigs databases 
```
## Creating the anvi’o contigs databases
# one cmd for all (multiple files)
(anvio-7.1) jiang@azur:~/user_name/your_place$ for f in *_headfix.fasta; do anvi-gen-contigs-database -f $f -o CD_${f%_*}_CD.db; done
# one by one if you like


## Annotating your contigs databases
# anvi-run-hmms
(anvio-7.1) jiang@azur:~/user_name/your_place$ for f in *_CD.db; do anvi-run-hmms -c $f -T n; done (n: the highest sequence number)
# anvi-run-ncbi-cogs (COG20)
(anvio-7.1) jiang@azur:~/user_name/your_place$ for f in *_CD.db; do anvi-run-ncbi-cogs -c $f -T 20; done
# Run KOfam HMMs
(anvio-7.1) jiang@azur:~/user_name/your_place$ for f in *_CD.db; do anvi-run-kegg-kofams -c $f -T 20; done
```

#### 01-03 Generating an anvio genomes storage
#external_database_path.txt
name	contigs_db_path

```
(anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-gen-genomes-storage -e external_database_path.txt -o GDB_XXXX_GENOMES.db
```
		
#### 01-04 Running the pangenome analysis
```
# slow mode, but recommend
(anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-pan-genome -g GDB_XXXX_GENOMES.db -n PROJECT_XXXX -T 20 --enforce-hierarchical-clustering --use-ncbi-blast
# fast mode
(anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-pan-genome -g GDB_XXXX_GENOMES.db -n PROJECT_XXXX -T 20 --enforce-hierarchical-clustering
```
>*--use-ncbi-blast (default: False) This program uses DIAMOND by default;*

>*--enforce-hierarchical-clustering (default: False)/--skip-hierarchical-clustering (default: False)*	

#### 01-05 Displaying the pangenome
```
# before displaying remotely, re-connect the server by 'ssh -L 8080:localhost:8080 xxxx'
(anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-display-pan -p PROJECT_XXXX/PROJECT_XXXX-PAN.db -g G_XXXX_GENOMES.db --server-only -P 8080
```

#### 01-06 Default summary
```
 # check collections
 (anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-summarize  -p PROJECT_XXXX/PROJECT_XXXX-PAN.db -g G_XXXX_GENOMES.db --list-collections
 # add a 'DEFAULT' collection for pangenome
 (anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-script-add-default-collection  -p PROJECT_XXXX/PROJECT_XXXX-PAN.db 
 # summarize the 'DEFAULT' collection
 (anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-summarize -p PROJECT_XXXX/PROJECT_XXXX-PAN.db -g G_XXXX_GENOMES.db  -C DEFAULT -o SUMMARY-XXXX-default
```

#### 01-07 Optional choice
```
 #  ANI calculation
(anvio-7.1) jiang@azur:~/user_name/your_place$ anvi-compute-genome-similarity --program pyANI -i txt-internal-genomes.txt -p PROJECT-Sulfitobacter-PAN/PROJECT-Sulfitobacter-PAN-PAN.db -o pyANI-ANIb -T 20
```


### Usage02: [Metagenomics]()


### Usage03: [Meta-pan genomics]()
