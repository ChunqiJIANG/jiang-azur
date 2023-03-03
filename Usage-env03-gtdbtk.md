
This page introduces the usages in the environment of **'gtdbtk-2.1.1'**.

<p align="right"> Updated on 2023-03-03 </p>

#### Usage description
Usage text 
```
  # annotation
  (environment) ~/location$ command line
```


### Check and Activate the environment first
```
  # don't forget to change to your directory first
  (base) jiang@azur:~$ cd user_name
  
  # check current enviroment at the beginning of command line, like the 'base' here
  (base) jiang@azur:~/user_name$
  # if not thetargete d enviroment, activate it by 'conda activate'
  (base) jiang@azur:~/user_name$ conda activate gtdbtk-2.1.1
  # check again at the beginning, now is 'gtdbtk-2.1.1', good to go
  (gtdbtk-2.1.1) jiang@azur:~/user_name$ 
```

## Let's start !

### Usage01: classify workflow
The classify workflow will run the following steps: 
- ani_screen 
- **identify**, 
- **align**
- **classify**
 Details here https://ecogenomics.github.io/GTDBTk/commands/classify_wf.html#classify-wf

```
  (gtdbtk-2.1.1) jiang@azur:~/user_name$  gtdbtk classify_wf --genome_dir <genomes_dir> --extension <fasta/fna/fa/gz> --out_dir <output_dir> --cpu <num>

```
The taxonomic classification of each bacterial and archaeal genome is contained in the *[prefix].[domain].summary.tsv* output files.


### Usage02: de_novo workflow
The de novo workflow will run the following steps: **identify**, **align**, **infer**, **root**, and **decorate**. Details here https://ecogenomics.github.io/GTDBTk/commands/de_novo_wf.html#de-novo-wf
> The de novo workflow infers new bacterial and archaeal trees containing all user supplied and GTDB-Tk reference genomes. The ***classify workflow*** is recommended for obtaining taxonomic classifications, and this workflow only recommended if a de novo domain-specific trees are desired. 

```
  (gtdbtk-2.1.1) jiang@azur:~/user_name$  gtdbtk de_novo_wf --genome_dir <genomes_dir>  --outgroup_taxon <outgroup> --out_dir <output_dir>

  # example
  (gtdbtk-2.1.1) jiang@azur:~/user_name$  gtdbtk de_novo_wf --genome_dir genomes/ --outgroup_taxon p__Chloroflexota --bacteria  --taxa_filter p__Firmicutes --out_dir  de_novo_output

  # skip GTDB reference genomes ( requires --custom_taxonomy_file for outgrouping, you can retrieve it from classify step)
  (gtdbtk-2.1.1) jiang@azur:~/user_name$  gtdbtk de_novo_wf --genome_dir genomes/ --outgroup_taxon p__Customphylum --bacteria --custom_taxonomy_file custom_taxonomy.tsv --out_dir de_novo_output

```
