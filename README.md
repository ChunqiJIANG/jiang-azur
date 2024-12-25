
# This is a repository[^1] for internal usage ([Yoshizawa Group](https://genedynamics.aori.u-tokyo.ac.jp/)) in server space 'jiang@azur' *(access necessary)*.

<p align="right"> Start from 2023-02-28 </p>

> For access to 'jiang@azur' at the first time, please contact Jiang[^2].

*It is possible to refer to all the usages listed here on your own terminal if you have a similar environment and OS. Free to ask if have any questions.*


[^1]: All Rights Reserved.
[^2]: You should know where to find him^.^

---
#### Notes:

- ALWAYS enter your directory first ```cd user_name``` before any action!

- DO NOT install, update or modify anything without contact! 

- CONTACT first if you want new packages or environments! 


#### Currently released environments(env):
<p align="right"> Recent update on 2024-12-27 </p>

|  <env_id>  |  <env_name>  |  <create_date>  |  <update_date>  |  <env_description>  | 
|  ----  |  ----  |  ----  |  ----  |  ----  |
|  00  | base          | 2023-02-28 |            | basic environment, common usages |
|  01  | anvio-7.1     | 2023-02-28 |            | **anvi'o** for pangenome / metagenome analyses |
|  02  | assembly      | 2023-02-28 |            | tools for genome assembly: **seqkit, fastqc, unicycler, flye, medaka, pilon ...** |
|  03  | gtdbtk-2.1.1  | 2023-02-28 |   env 12   | **gtdbtk** tool for GDTB |
|  04  | checkm        | 2023-02-28 |   env 13   | genome quialty check (**checkm** or **gunc**), completeness and contamination|
|  05  | pydata        | 2023-02-28 |            | data analysis using python  |
|  06  | r-env         | 2023-02-28 |            | data analysis using R  |
|  07  | unicycler050  | 2023-02-13 |            | only unicycler 0.5.0 |
|  08  | qiime2-2023.2 | 2023-03-10 |   env 14  | for **qiime2** analyses |
|  09  | ncbi          | 2023-03-28 | 2023-07-12 | local blast using **blast+** or **diamond** |
|  10  | prokka        | 2023-07-03 |            | rapid prokaryotic genome annotation |
|  11  | rna-seq       | 2023-11-02 |            | RNA seq analysis: **Trimmomatic, Trinity** |
|  12  | gtdbtk-2.4.0  | 2024-05-14 | 2024-12-25 | **gtdbtk-2.4.0 R220 (ver202404)**, iqtree, fasttree, mafft, raxml-ng|
|  13  | checkm2       | 2024-06-04 |            | MAGs quality check: **checkM2, QUAST, GUNC** |
|  14  | qiime2-amplicon-2024.5 | 2024-06-05 |            | The **2024.5 release** of the QIIME 2 **Amplicon** Distribution includes the QIIME 2 framework, q2cli and the following plugins: **q2-alignment, q2-composition, q2-cutadapt, q2-dada2, ...** |
|  15  | qiime2-metagenome-2024.5 | 2024-06-07 |            | The **2024.5 release** of the QIIME 2 **Metagenome** Distribution includes the QIIME 2 framework, q2cli (a QIIME 2 command-line interface) and the following plugins: **q2-assembly, q2-composition, q2-cutadapt, q2-demux, ...** |
|  16  | metawrap          | 2024-06-10 | 2024-10-18 | **MetaWRAP** environment, a flexible pipeline for genome-resolved metagenomic data analysis: metawrap-mg, blast, concoct, maxbin2, megahit, metabat2, … **Databases installed**: checkm, ncbi_nt, ncbi_tax, SILVA 16S  |
|  17  | anvio-8           | 2024-06-18 |            | **anvi'o v8** for pangenome / metagenome analyses: hmmer, diamond, blast, spades, megahit, prodigal, fasttree, fastani, concoct, coverM, kaiju,seqkit … **Databases installed**: SCG v214.1, COG20, KEGG_20230922, cazymes V12 |
|  18  | metaplatanus      | 2024-09-12 |            | **MetaPlatanus** is a de novo assembler for metagenome (microbiome): metaplatanus, megahit, metabat2, minimap2, samtools, seqkit… |
|  19  | meta              | 2024-09-30 |            | An optional env for meta analysis: **metaphlan** (db vJun23_202403) |
|  20  | mtb               | 2024-12-17 |            | For **MTB** related analyses: fegenie, magcluster … |
|  21  | hocort            | 2024-12-25 |            | Removes specific organisms from sequencing reads on Linux: blast, bowtie2, bwa-mem2, **hocort**, kraken2, minimap2, samtools... |
|  --  | -          | - | -          | - |


***Shortcuts：***

- Basic information on this space and the general list of environments please check [here](https://github.com/ChunqiJIANG/jiang-azur/blob/main/Info_system.md).  

- Detailed list of environments and packages please check [here](https://github.com/ChunqiJIANG/jiang-azur/blob/main/List_environments.md).  


***Usages：***

- [Connection](https://github.com/ChunqiJIANG/jiang-azur/blob/main/First-connection-before-use.md)
  - 2023-03-06 created
- [Env00-base](https://github.com/ChunqiJIANG/jiang-azur/blob/main/usages/Usage-env00-base.md)
  - Introduction of common/basic commands
- [Env01-anvio-7.1](https://github.com/ChunqiJIANG/jiang-azur/blob/main/usages/Usage-env01-anvio-7.1.md)
  - 2023-05-15 updated
  - 2023-03-02 created
- [Env02-assembly](https://github.com/ChunqiJIANG/jiang-azur/blob/main/usages/Usage-env02-assembly.md)
  - 2023-05-15 updated
  - 2023-03-07 created
- [Env03-gtdbtk-2.1.1](https://github.com/ChunqiJIANG/jiang-azur/blob/main/usages/Usage-env03-gtdbtk-2.1.1.md)
  - 2023-03-06 updated
  - 2023-03-02 created
- [Env04-checkm](https://github.com/ChunqiJIANG/jiang-azur/blob/main/usages/Usage-env04-checkm.md)
  - 2023-07-06 **'gunc'** was added for MAG check
  - 2023-03-06 created
- [Env05-pydata]()
- [Env06-r-env]()
- [Env07-unicycler050]()
- [Env08-qiime2-2023.2]()
- [Env09-ncbi](https://github.com/ChunqiJIANG/jiang-azur/blob/main/usages/Usage-env09-ncbi.md)
  - 2023-07-12 **'DIAMOND'** was added for large scale blast
- [Env10-prokka](https://github.com/ChunqiJIANG/jiang-azur/blob/main/usages/Usage-env10-prokka.md)
  - 2023-07-28 created
