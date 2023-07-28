
This page introduces the usages in the environment of **'prokka'**.

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
  (base) jiang@azur:~/user_name$ conda activate prokka
  # check again at the beginning, now it is 'prokka', good to go
  (prokka) jiang@azur:~/user_name$ 
```

---
# Let's start!

## [Prokka: rapid prokaryotic genome annotation](https://github.com/tseemann/prokka)
>  Prokka, a command line software tool to fully annotate a draft bacterial genome in about 10 min on a typical desktop computer. It produces standards-compliant output files for further analysis or viewing in genome browsers.
>> Full details can be found [here](https://academic.oup.com/bioinformatics/article/30/14/2068/2390517).

#quick and simple
```
(prokka) jiang@azur:~/user_name$ prokka GENOME.fna
```
#set the names of the output files
```
(prokka) jiang@azur:~/user_name$ prokka GENOME.fna -o PROKKA_OUT --prefix NAME
```
#set the names of the output files
```
(prokka) jiang@azur:~/user_name$ prokka GENOME.fna -o PROKKA_OUT --prefix NAME
```
#add locus_tag prefix
```
(prokka) jiang@azur:~/user_name$ prokka GENOME.fna -o PROKKA_OUT --prefix NAME --locustag XXX
```
#Force Genbank/ENA/DDJB compliance:
``` 
(prokka) jiang@azur:~/user_name$ prokka GENOME.fna --compliant --outdir PROKKA_OUT --prefix NAME --locustag XXX
```
  *'--cpu n' is welcomed (default 8)*


### Helpful for multiple files at one time
#use your file names for the names of output and locus_tag
``` 
(prokka) jiang@azur:~/user_name$ for f in *.fna; do prokka $f --outdir ../PROKKA_OUT/Prokka_${f%.*} --prefix ${f%.*} --locustag ${f%.*} --addgenes --cpus 20; done
```
