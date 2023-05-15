This page introduces the usages in the environment of **'assembly'**.

<p align="right"> Updated on 2023-04-19 </p>

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
  # check again at the beginning, now it is 'assembly', good to go
  (assembly) jiang@azur:~/user_name$ 
```

## Let's start !!!
The usages of each tool in this environment are listed:

### Remote copy upload
```
scp /path/to/file azur:~/path/to/directory/
scp -r azur:~/path/to/directory/ /Users/chunqijiang/Dropbox/Mine_UTokyo_AORI/local/ 
```


### check sequence features
```
seqkit watch --fields ReadLen T4_f2000_p90_b5M.fastq.gz -O len_T4_f2p9b5m.png
seqkit stat *.gz -a
```


### check data quality
```
fastqc T4_f1000_p90.fastq.gz -o fastqc -t 10
```


### check genome quality (completeness)
```
checkm lineage_wf -t 40 -x fna path/to/fasta/directory/ /results/directory/
checkm taxonomy_wf phylum Chloroflexi -t 40 -x fna path/to/fasta/directory/ /taxowf-results/directory/
```


### filter long reads 
(priority: --target_bases > --keep_percent > --min_length)
```
filtlong --keep_percent 90 --min_length 1000  N4_25.fastq.gz | gzip > QF_N4_25.fastq.gz
filtlong --keep_percent 90 --min_length 2000 --target_bases 500000000 T4.fastq.gz | gzip > T4_f2000_p90_b5M.fastq.gz
```


### filter short reads
```
fastp --in1 sample_R1.fastq.gz --in2 sample_R2.fastq.gz --out1 QC_sample_R1.fastq.gz --out2 QC_sample_R2.fastq.gz
fastp --in1 in.R1.fq.gz --in2 in.R2.fq.gz --out1 R1_trimmed.fq.gz --out2 R2_trimmed.fq.gz --thread 20 
fastp --in1  --in2  --out1 --out2  --thread 20
```


### unicycler
```
# hybrid 
unicycler -1 QF_N3_17_Read1.fq.gz -2 QF_N3_17_Read2.fq.gz -l QF_N3_17_v2.fastq.gz -t 30 -o Unicycler048_N3_17
# short only
unicycler -1 QF_short_R1.fq.gz -2 QF_short_R2.fq.gz  -o unicycler_short_SAMPLE --threads 20 --no_correct --no_pilon
```


### flye
```
flye --nano-raw QF_N4_25_p60-l9000.fastq.gz --out-dir Flye_N4_25_v1 -t 20
```


### medaka
```
medaka_consensus -i QF_N4_25_1G.fastq.gz -d assembly_N4_25_flye.fasta -o N4_25_Medaka -t 10
```


### pilon
```
# step by step
		# create an index for the reference genome (fasta) with Bowtie2
		bowtie2-build Medaka_SAMPLE/consensus.fasta index_SAMPLE (--threads 10)
		# align the short reads to the reference genome
		bowtie2 -x index_SAMPLE -1 QF_SAMPLE_Read1.fq.gz -2 QF_SAMPLE_Read2.fq.gz -S SAMPLE.sam --threads 10
		# convert the SAM file into a BAM file 
		samtools view -bS SAMPLE.sam > SAMPLE.bam --threads 10
		# convert the BAM file to a sorted BAM file
		samtools sort SAMPLE.bam -o SAMPLE.sorted.bam --threads 10
		# index the sorted BAM file (  -@ Sets the number of threads)
		samtools index -@ 10 SAMPLE.sorted.bam
		# polish with Pilon (--threads no longer support)
		pilon --genome Medaka_SAMPLE/consensus.fasta --bam SAMPLE.sorted.bam --changes --outdir Pilon_SAMPLE 
		## repeat ...

# one script for all 
	# default iterate 5 times, you can change "5" to any number you like, but less than 10 is recommended.
 	/home/jiang/user_jiang/scripts/script_pilon-polish_iterate_azur_T20_ver3.sh -p PilonSHv3_SAMPLE_n5 -i SAMPLE.fasta -1 QF_SAMPLE_Read1.fq.gz -2 QF_SAMPLE_Read2.fq.gz -n 5

```


### check length
```
# Print sequence length, GC content, and only print names (no sequences), we could also print title line by flag -H.
	seqkit fx2tab -l -g -n -i -H assembly.fasta
# Sort sequences by id/name/sequence/length.
	seqkit sort -l assembly.fasta > assembly.sorted.fasta (short to long)
	seqkit sort -l -r assembly.fasta > assembly.sorted.fasta (long to short)
  
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
