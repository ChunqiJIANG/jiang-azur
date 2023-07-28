
This page introduces the usages in the environment of **'ncbi'**.

<p align="right"> Updated on 2023-07-28 </p>

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
  
  # check the current environment at the beginning of the command line, like the 'base' here
  (base) jiang@azur:~/user_name$
  # if not the target environment, activate it by 'conda activate'
  (base) jiang@azur:~/user_name$ conda activate ncbi
  # check again at the beginning, now it is 'ncbi', good to go
  (ncbi) jiang@azur:~/user_name$ 
```
---
Here mainly introduce two tools for local blast:
- NCBI blast+
- DIAMOND  *(optimized for large input files of >1 million proteins)*


### NCBI blast+
#for protein
```
(ncbi) jiang@azur:~/user_name$ makeblastdb -dbtype prot -hash_index -parse_seqids -in DATABASE.faa
(ncbi) jiang@azur:~/user_name$ blastp -evalue 0.01 -outfmt 6 -db DATABASE.faa -query QUERY.faa  -out OUTPUT.txt 
```
#for nucleic acid
```
(ncbi) jiang@azur:~/user_name$ makeblastdb -dbtype nucl -hash_index -parse_seqids -in DATABASE.fna
(ncbi) jiang@azur:~/user_name$ blastn -evalue 0.01 -outfmt 6 -db DATABASE.fna -query QUERY.fna  -out OUTPUT.txt 
```
**Format of outfmt6:*

query acc.ver, subject acc.ver, % identity, alignment length, mismatches, gap opens, q. start, q. end, s. start, s. end, evalue, bit score


### DIAMOND
#create a diamond-formatted database file
```
(ncbi) jiang@azur:~/user_name$ diamond makedb --in reference.fasta -d reference
```
#run a blast search 
```
# blastp mode
(ncbi) jiang@azur:~/user_name$ diamond blastp -d reference -q queries.fasta -o matches.tsv
# blastx mode
(ncbi) jiang@azur:~/user_name$ diamond blastx -d reference -q reads.fasta -o matches.tsv
```
