This page introduces the usages in the environment of **'assembly'**.

<p align="right"> Updated on 2023-06-30 </p>

#### Usage description
Usage text 
```
  # annotation line
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
  # check again at the beginning, now it is 'assembly', good to go
  (assembly) jiang@azur:~/user_name$ 
```

---
#### Prep: Remote upload/download
```
## Follow commands should be performed at your local terminal (not the server side, open a new terminal window when you need)

## upload to jiang@azur
# for file
$ scp -p 10022 /path/to/local/file jiang@157.82.133.226:~/path/to/save/directory/
# for folder
$ scp -r -p 10022 /path/to/local/folder/ jiang@157.82.133.226:~/path/to/save/directory/

## download from jiang@azur
# for file
$ scp -p 10022 jiang@157.82.133.226:~/path/to/file/ /path/to/local/directory/ 
# for folder
$ scp -r -p 10022 jiang@157.82.133.226:~/path/to/folder/ /path/to/local/directory/ 
```

---
## Let's start !!!

There are three strategies for genome assembly, please choose the suitable one according to your conditions(data).
- S1-**Short**  (only use short reads, like Illumina data)
- S2-**Long** (only use long reads, like Nanopore/PacBio data)  
- S3-**Hybrid** (use both short and long reads)

### S1-**Short** Strategy
#### S1.1 filter reads (short) by 'fastp'
```
# example 1 (default)
(assembly) jiang@azur:~/user_name$ fastp --in1 sample_short_R1.fastq.gz --in2 sample_short_R2.fastq.gz --out1 QF_sample_short_R1.fastq.gz --out2 QF_sample_short_R2.fastq.gz
# example 2 (set threads number, max = 16)
(assembly) jiang@azur:~/user_name$ fastp --in1 sample_short_R1.fastq.gz --in2 sample_short_R2.fastq.gz --out1 QF_sample_short_R1.fastq.gz --out2 QF_sample_short_R2.fastq.gz --thread 16

# example 3 (simple use by copy/paste, exchange the 'xxx' parts with yours)
(assembly) jiang@azur:~/user_name$ fastp --in1 xxx --in2 xxx --out1 xxx --out2 xxx --thread 16
```
#### S1.2 assembly by 'unicycler' using short-only method
```
# example 1 (default)
(assembly) jiang@azur:~/user_name$ unicycler -1 QF_sample_short_R1.fastq.gz -2 QF_sample_short_R2.fastq.gz  -o unicycler_short_NAME --threads 20 --no_correct --no_pilon

# example 2 (simple use by copy/paste, exchange the 'xxx' parts with yours)
(assembly) jiang@azur:~/user_name$ unicycler -1 xxx -2 xxx -o unicycler_xxx --threads 20 --no_correct --no_pilon
```


### S2-**Long** Strategy
#### S2.1 filter reads (long) by 'filtlong'
(priority: --target_bases > --keep_percent > --min_length)
```
# example 1 (common use: only keep 90% data with minimum length at 1000bp )
(assembly) jiang@azur:~/user_name$ filtlong --keep_percent 90 --min_length 1000  sample_long.fastq.gz | gzip > QF_sample_long.fastq.gz

# example 2 (custom use: only keep 90% data with minimum length at 2000bp and total size at 5Mbp)
(assembly) jiang@azur:~/user_name$ filtlong --keep_percent 90 --min_length 2000 --target_bases 500000000 sample_long.fastq.gz | gzip > QF_sample_long_f2000_p90_b5M.fastq.gz
```
#### S2.2 assembly by 'flye'
```
(assembly) jiang@azur:~/user_name$ flye --nano-raw QF_sample_long.fastq.gz --out-dir Flye_NAME -t 20
```
#### S2.3 polish by 'medaka'
```
(assembly) jiang@azur:~/user_name$ medaka_consensus -i QF_N4_25_1G.fastq.gz -d Flye_NAME/assembly.fasta -o Medaka_NAME -t 20
```



### S3-**Hybrid** Strategy
#### S3.1 filter reads (short) by 'fastp'
```
# example 1 (default)
(assembly) jiang@azur:~/user_name$ fastp --in1 sample_short_R1.fastq.gz --in2 sample_short_R2.fastq.gz --out1 QF_sample_short_R1.fastq.gz --out2 QF_sample_short_R2.fastq.gz
# example 2 (set threads number, max = 16)
(assembly) jiang@azur:~/user_name$ fastp --in1 sample_short_R1.fastq.gz --in2 sample_short_R2.fastq.gz --out1 QF_sample_short_R1.fastq.gz --out2 QF_sample_short_R2.fastq.gz --thread 16

# example 3 (simple use by copy/paste, exchange the 'xxx' parts with yours)
(assembly) jiang@azur:~/user_name$ fastp --in1 xxx --in2 xxx --out1 xxx --out2 xxx --thread 16
```
#### S3.2 filter reads (long) by 'filtlong'
(priority: --target_bases > --keep_percent > --min_length)
```
# example 1 (common use: only keep 90% data with minimum length at 1000bp )
(assembly) jiang@azur:~/user_name$ filtlong --keep_percent 90 --min_length 1000  sample_long.fastq.gz | gzip > QF_sample_long.fastq.gz

# example 2 (custom use: only keep 90% data with minimum length at 2000bp and total size at 5Mbp)
(assembly) jiang@azur:~/user_name$ filtlong --keep_percent 90 --min_length 2000 --target_bases 500000000 sample_long.fastq.gz | gzip > QF_sample_long_f2000_p90_b5M.fastq.gz
```
#### S3.3 assembly by 'unicycler' using hybrid method
```
(assembly) jiang@azur:~/user_name$ unicycler -1 QF_sample_short_R1.fastq.gz -2 QF_sample_short_R2.fastq.gz -l QF_sample_long.fastq.gz -t 20 -o Unicycler048_Hybrid_NAME
```


### Further Polish by 'pilon'
```
# step by step
		# create an index for the reference genome (fasta) with Bowtie2
		$ bowtie2-build Medaka_SAMPLE/consensus.fasta index_SAMPLE (--threads 10)
		# align the short reads to the reference genome
		$ bowtie2 -x index_SAMPLE -1 QF_SAMPLE_Read1.fq.gz -2 QF_SAMPLE_Read2.fq.gz -S SAMPLE.sam --threads 10
		# convert the SAM file into a BAM file 
		$ samtools view -bS SAMPLE.sam > SAMPLE.bam --threads 10
		# convert the BAM file to a sorted BAM file
		$ samtools sort SAMPLE.bam -o SAMPLE.sorted.bam --threads 10
		# index the sorted BAM file (  -@ Sets the number of threads)
		$ samtools index -@ 10 SAMPLE.sorted.bam
		# polish with Pilon (--threads no longer support)
		$ pilon --genome Medaka_SAMPLE/consensus.fasta --bam SAMPLE.sorted.bam --changes --outdir Pilon_SAMPLE 
		## repeat ...

# one script for all 
	# default iterate 5 times, you can change "5" to any number you like, but less than 10 is recommended.
 	$ /home/jiang/user_jiang/scripts/script_pilon-polish_iterate_azur_T20_ver3.sh -p PilonSHv3_SAMPLE_n5 -i SAMPLE.fasta -1 QF_SAMPLE_Read1.fq.gz -2 QF_SAMPLE_Read2.fq.gz -n 5

```

### Optional usages
#### check sequence features
```
# example 1 (check length distribution)
(assembly) jiang@azur:~/user_name$ seqkit watch --fields ReadLen T4_f2000_p90_b5M.fastq.gz -O len_T4_f2p9b5m.png

# example 2 (list all information for all .gz files)
(assembly) jiang@azur:~/user_name$ seqkit stat *.gz -a
```

#### check data quality
```
# example 1 
(assembly) jiang@azur:~/user_name$ fastqc T4_f1000_p90.fastq.gz -o fastqc -t 10
```

#### check genome quality (completeness)

```
# example 1 
(assembly) jiang@azur:~/user_name$ checkm lineage_wf -t 40 -x fna path/to/fasta/directory/ /results/directory/

# example 2
(assembly) jiang@azur:~/user_name$ checkm taxonomy_wf phylum Chloroflexi -t 40 -x fna path/to/fasta/directory/ /taxowf-results/directory/
```

#### check length
```
# Print sequence length, GC content, and only print names (no sequences), we could also print title line by flag -H.
	(assembly) jiang@azur:~/user_name$ seqkit fx2tab -l -g -n -i -H assembly.fasta
# Sort sequences by id/name/sequence/length.
	# short to long
	(assembly) jiang@azur:~/user_name$ seqkit sort -l assembly.fasta > assembly.sorted.fasta 
	# long to short
	(assembly) jiang@azur:~/user_name$ seqkit sort -l -r assembly.fasta > assembly.sorted.fasta 
  
Flags:
  -b, --by-bases                by non-gap bases
  -l, --by-length               by sequence length
  -n, --by-name                 by full name instead of just id
  -s, --by-seq                  by sequence
  -G, --gap-letters string      gap letters (default "- \t.")
  -h, --help                    help for sort
  -i, --ignore-case             ignore case
  -k, --keep-temp               keep tempory FASTA and .fai file when using 2-pass mode
  -N, --natural-order           sort in natural order, when sorting by IDs/full name
  -r, --reverse                 reverse the result
  -L, --seq-prefix-length int   length of sequence prefix on which seqkit sorts by sequences (0 for whole sequence) (default 10000)
  -2, --two-pass                two-pass mode read files twice to lower memory usage. (only for FASTA format)

```
