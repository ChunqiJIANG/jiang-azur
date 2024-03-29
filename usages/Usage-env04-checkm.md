
This page introduces the usages in the environment of **'checkm'**.

<p align="right"> Updated on 2023-03-06 </p>

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
  (base) jiang@azur:~/user_name$ conda activate checkm
  # check again at the beginning, now it is 'checkm', good to go
  (checkm) jiang@azur:~/user_name$ 
```

# Let's start with [CheckM](https://ecogenomics.github.io/CheckM/) v1.2.2 !

> CheckM provides a set of tools for assessing the quality of genomes recovered from isolates, single cells, or metagenomes. It provides robust estimates of genome completeness and contamination by using collocated sets of genes that are ubiquitous and single-copy within a phylogenetic lineage. Assessment of genome quality can also be examined using plots depicting key genomic characteristics (e.g., GC, coding density) which highlight sequences outside the expected distributions of a typical genome. CheckM also provides tools for identifying genome bins that are likely candidates for merging based on marker set compatibility, similarity in genomic characteristics, and proximity within a reference genome tree.
>> Full details can be found [here](https://github.com/Ecogenomics/CheckM/wiki).

Here mainly introduce two workflows for genome quality check:
- Lineage-specific Workflow (quality estimates with lineage-specific markers)
- Taxonomic-specific Workflow (quality estimates with taxonomic-specific markers)

---

### Usage01:lineage workflow
Regardless of taxonomy known/unknown
```
(checkm) jiang@azur:~/user_name$ checkm lineage_wf -t 20 -x fna putative/genomes/directory/ checkm/results/directory/ | tee checkm-linewf-results.txt

# example
(checkm) jiang@azur:~/user_name$ checkm lineage_wf -t 20 -x fna ./genomes/ ./checkm-linewf-results | tee log-checkm-linewf-results.txt
```
*results will be shown at the bottom of the screen and the txt file.  


#example
|Bin Id|Marker lineage|	#genomes|	#markers|	#marker sets|	0|	1|	2|	3|	4|	5+|	Completeness|	Contamination|	Strain heterogeneity| 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|GCA_003967575.1_ASM396757v1_genomic|	Chloroflexi(1)|	20|	225|	149|	0|	200|	22|	2|	1|	0	|100|	12.69|	0|  
|GCA_003268475.1_ASM326847v1_genomic|	Chloroflexi(1)|	20|	225|	149|	0|	224|  1|	0|	0|	0|100|	0.67|	0|  
|...|...|...|...|...|...|...|...|...|...|...|...|...|...| 

> - bin id: unique identifier of genome bin (derived from input fasta file)
> - **marker lineage**: indicates the taxonomic rank of the lineage-specific marker set used to estimated genome completeness, contamination, and strain heterogeneity. More detailed information about the placement of a genome within the reference genome tree can be obtained with the tree_qa command. The UID indicates the branch within the reference tree used to infer the marker set applied to estimate the bins quality.
> - genomes: number of reference genomes used to infer the lineage-specific marker set
> - markers: number of marker genes within the inferred lineage-specific marker set
> - marker sets: number of co-located marker sets within the inferred lineage-specific marker set
> - 0-5+: number of times each marker gene is identified
> - **completeness**: estimated completeness of genome as determined from the presence/absence of marker genes and the expected collocalization of these genes
> - **contamination**: estimated contamination of genome as determined by the presence of multi-copy marker genes and the expected collocalization of these genes
> - strain heterogeneity: estimated strain heterogeneity as determined from the number of multi-copy marker pairs which exceed a specified amino acid identity threshold (default = 90%). High strain heterogeneity suggests the majority of reported contamination is from one or more closely related organisms (i.e. potentially the same species), while low strain heterogeneity suggests the majority of contamination is from more phylogenetically diverse sources. 


### Usage02: taxonomy workflow
Only if taxonomy known
```
# specify the 'rank' and 'taxon',  available ones are listed in the next section "checkm available taxon_list"
(checkm) jiang@azur:~/user_name$ checkm taxonomy_wf <rank> <taxon> -t 20 -x fna putative/genomes/directory/ checkm/results/directory/ | tee checkm-taxowf-results.txt

# example
(checkm) jiang@azur:~/user_name$ checkm taxonomy_wf phylum Chloroflexi -t 20 -x fna ./genomes/ ./checkm-taxowf-results | tee log-checkm-taxowf-results.txt
``` 
*similar format results will be shown at the bottom of the screen and the txt file.


# New tool with [GUNC](https://grp-bork.embl-community.io/gunc/index.html)
> Genome UNClutterer (GUNC) is a tool for detection of chimerism and contamination in prokaryotic genomes resulting from mis-binning of genomic contigs from unrelated lineages. It does so by applying an entropy based score on taxonomic assignment and contig location of all genes in a genome.
>> Full details can be found [here](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02393-0).

---
### Usage
#### Step01: Run chimerism detection
```
# quick start
(checkm) jiang@azur:~/user_name$ gunc run -i genome.fa -r ~/mambaforge-pypy3/envs/checkm/gunc_db_progenomes2.1.dmnd

# for directory(GENOME/) with multiple fasta files (*.fa)
(checkm) jiang@azur:~/user_name$ gunc run -d GENOME/ -r ~/mambaforge-pypy3/envs/checkm/gunc_db_progenomes2.1.dmnd -t 20 -o GUNC_OUT/
# for directory(GENOME/) with multiple fasta files (*.fna or others)
(checkm) jiang@azur:~/user_name$ gunc run -d GENOME/ -e .fna -r ~/mambaforge-pypy3/envs/checkm/gunc_db_progenomes2.1.dmnd -t 20 -o GUNC_OUT/
```
*results can be seen in the '*.tsv' file.

#### Step02: Produce an interactive plot using the output from Step01
```
(checkm) jiang@azur:~/user_name$ gunc plot -d GUNC_OUT/diamond_output/*.out 
```
*results can be seen in the '*.viz.html' file.


---
## Checkm available taxon_list
```
(checkm) jiang@azur:~/user_name$ checkm taxon_list
[2023-02-17 15:07:59] INFO: CheckM v1.2.2
[2023-02-17 15:07:59] INFO: checkm taxon_list
[2023-02-17 15:07:59] INFO: CheckM data: /home/jiang/mambaforge-pypy3/envs/checkm/checkm_data
[2023-02-17 15:07:59] INFO: [CheckM - taxon_list] Listing available taxonomic-specific marker sets.

------------------------------------------------------------------------------------------------------------
  Rank      Taxon                                               # genomes   # marker genes   # marker sets  
------------------------------------------------------------------------------------------------------------
  life      Prokaryote                                             5656           56               24       
  domain    Archaea                                                207           149              107       
  domain    Bacteria                                               5449          104               58       
  phylum    Acidobacteria                                           15           399              276       
  phylum    Actinobacteria                                         731           204              119       
  phylum    Aquificae                                               18           486              369       
  phylum    Bacteroidetes                                          419           286              195       
  phylum    Chlamydiae                                              64           455              185       
  phylum    Chlorobi                                                12           612              333       
  phylum    Chloroflexi                                             20           225              149       
  phylum    Crenarchaeota                                           54           217              168       
  phylum    Cyanobacteria                                          129           472              368       
  phylum    Deferribacteres                                         6            564              339       
  phylum    Deinococcus-Thermus                                     40           528              359       
  phylum    Dictyoglomi                                             2            1060             103       
  phylum    Euryarchaeota                                          146           188              125       
  phylum    Firmicutes                                             1349          172               99       
  phylum    Fusobacteria                                            32           289              159       
  phylum    Ignavibacteriae                                         2            1003             383       
  phylum    Nitrospirae                                             4            676              379       
  phylum    Planctomycetes                                          11           360              256       
  phylum    Proteobacteria                                         2343          182              119       
  phylum    Spirochaetes                                            71           218              127       
  phylum    Synergistetes                                           13           452              160       
  phylum    Tenericutes                                            119           177              105       
  phylum    Thaumarchaeota                                          4            548              265       
  phylum    Thermodesulfobacteria                                   5            813              440       
  phylum    Thermotogae                                             14           460              267       
  phylum    Verrucomicrobia                                         12           346              245       
  class     Acidobacteriia                                          10           572              395       
  class     Aciduliprofundum                                        2            763              130       
  class     Actinobacteria                                         729           205              119       
  class     Alphaproteobacteria                                    648           225              148       
  class     Aquificae                                               18           486              369       
  class     Archaeoglobi                                            5            619              392       
  class     Bacilli                                                821           250              136       
  class     Bacteroidetes Order II. Incertae sedis                  5            801              466       
  class     Bacteroidia                                            211           401              266       
  class     Betaproteobacteria                                     322           387              234       
  class     Chlamydiia                                              64           455              185       
  class     Chlorobia                                               12           612              333       
  class     Chloroflexi                                             9            452              315       
  class     Chroococcales                                           55           490              378       
  class     Clostridia                                             446           196              110       
  class     Cytophagia                                              48           438              328       
  class     Deferribacteres                                         6            564              339       
  class     Dehalococcoidetes                                       5            755               54       
  class     Deinococci                                              40           528              359       
  class     Deltaproteobacteria                                     93           197              125       
  class     Dictyoglomia                                            2            1060             103       
  class     Epsilonproteobacteria                                  111           445              271       
  class     Erysipelotrichi                                         18           301              159       
  class     Flavobacteriia                                         126           321              202       
  class     Fusobacteriia                                           32           289              159       
  class     Gammaproteobacteria                                    1167          280              178       
  class     Halobacteria                                            59           367              241       
  class     Holophagae                                              2            883              459       
  class     Ignavibacteria                                          2            1003             383       
  class     Methanobacteria                                         12           513              264       
  class     Methanococci                                            16           610              439       
  class     Methanomicrobia                                         29           248              165       
  class     Mollicutes                                             119           177              105       
  class     Negativicutes                                           64           334              167       
  class     Nitrosopumilales                                        2            714              109       
  class     Nitrospira                                              4            676              379       
  class     Nostocales                                              18           732              508       
  class     Opitutae                                                4            676              427       
  class     Oscillatoriales                                         25           545              415       
  class     Planctomycetia                                          11           360              256       
  class     Pleurocapsales                                          6            840              624       
  class     Prochlorales                                            18           806              240       
  class     Solirubrobacterales                                     2            995              398       
  class     Sphingobacteriia                                        27           334              233       
  class     Spirochaetia                                            71           218              127       
  class     Stigonematales                                          5            1007             576       
  class     Synergistia                                             13           452              160       
  class     Thermococci                                             16           500              315       
  class     Thermodesulfobacteria                                   5            813              440       
  class     Thermomicrobia                                          2            933              336       
  class     Thermoplasmata                                          4            563              310       
  class     Thermoprotei                                            54           217              168       
  class     Thermotogae                                             14           460              267       
  class     Verrucomicrobiae                                        7            403              284       
  class     Zetaproteobacteria                                      2            1329             184       
  order     Acholeplasmatales                                       12           176               88       
  order     Acidilobales                                            3            558              236       
  order     Acidithiobacillales                                     6            795              254       
  order     Acidobacteriales                                        10           572              395       
  order     Aciduliprofundum                                        2            763              130       
  order     Actinomycetales                                        620           256              152       
  order     Aeromonadales                                           23           456              237       
  order     Alteromonadales                                        116           519              267       
  order     Aquificales                                             18           486              369       
  order     Archaeoglobales                                         5            619              392       
  order     Bacillales                                             331           319              134       
  order     Bacteroidales                                          210           402              267       
  order     Bacteroidetes Order II. Incertae sedis                  5            801              466       
  order     Bankia                                                  2            1388             332       
  order     Bdellovibrionales                                       5            466              290       
  order     Bifidobacteriales                                       77           464              220       
  order     Burkholderiales                                        194           427              216       
  order     Campylobacterales                                      107           458              273       
  order     Cardiobacteriales                                       3            807              349       
  order     Caulobacterales                                         15           644              366       
  order     Chlamydiales                                            64           455              185       
  order     Chlorobiales                                            12           612              333       
  order     Chloroflexales                                          8            501              342       
  order     Chromatiales                                            92           595              283       
  order     Chroococcales                                           55           490              378       
  order     Clostridiales                                          395           202              114       
  order     Coriobacteriales                                        23           304              155       
  order     Cytophagales                                            48           438              328       
  order     Deferribacterales                                       6            564              339       
  order     Dehalococcoidales                                       5            755               54       
  order     Deinococcales                                           17           563              388       
  order     Desulfobacterales                                       20           320              189       
  order     Desulfovibrionales                                      40           454              252       
  order     Desulfurellales                                         4            817              252       
  order     Desulfurococcales                                       14           287              213       
  order     Desulfuromonadales                                      13           550              283       
  order     Dictyoglomales                                          2            1060             103       
  order     Enterobacteriales                                      262           297              121       
  order     Entomoplasmatales                                       9            362              151       
  order     Erysipelotrichales                                      18           301              159       
  order     Flavobacteriales                                       126           321              202       
  order     Fusobacteriales                                         32           289              159       
  order     Halanaerobiales                                         7            504              189       
  order     Halobacteriales                                         59           367              241       
  order     Holophagales                                            2            883              459       
  order     Hydrogenophilales                                       3            1205             209       
  order     Ignavibacteriales                                       2            1003             383       
  order     Lactobacillales                                        490           335              183       
  order     Legionellales                                           23           545              269       
  order     Mariprofundales                                         2            1329             184       
  order     Methanobacteriales                                      12           513              264       
  order     Methanocellales                                         3            712              317       
  order     Methanococcales                                         16           610              439       
  order     Methanomicrobiales                                      11           465              230       
  order     Methanosarcinales                                       14           438              270       
  order     Methylococcales                                         15           684              348       
  order     Methylophilales                                         16           685              234       
  order     Mycoplasmatales                                         98           217              127       
  order     Myxococcales                                            8            534              348       
  order     Nautiliales                                             4            712              351       
  order     Neisseriales                                            69           658              446       
  order     Nitrosomonadales                                        7            851              314       
  order     Nitrosopumilales                                        2            714              109       
  order     Nitrospirales                                           4            676              379       
  order     Nostocales                                              18           732              508       
  order     Oceanospirillales                                       41           497              229       
  order     Opitutales                                              3            859              521       
  order     Oscillatoriales                                         25           545              415       
  order     Pasteurellales                                          83           767              440       
  order     Planctomycetales                                        10           370              265       
  order     Pleurocapsales                                          6            840              624       
  order     Prochlorales                                            18           806              240       
  order     Pseudomonadales                                        274           549              326       
  order     Rhizobiales                                            349           407              244       
  order     Rhodobacterales                                        104           504              327       
  order     Rhodocyclales                                           26           625              264       
  order     Rhodospirillales                                        57           367              228       
  order     Rickettsiales                                           81           325              210       
  order     Rubrobacterales                                         2            991              269       
  order     Selenomonadales                                         64           334              167       
  order     Solirubrobacterales                                     4            721              307       
  order     Sphingobacteriales                                      27           334              233       
  order     Sphingomonadales                                        31           490              270       
  order     Spirochaetales                                          71           218              127       
  order     Stigonematales                                          5            1007             576       
  order     Sulfolobales                                            20           499              141       
  order     Synergistales                                           13           452              160       
  order     Thermales                                               23           585              331       
  order     Thermoanaerobacterales                                  44           308              158       
  order     Thermococcales                                          16           500              315       
  order     Thermodesulfobacteriales                                5            813              440       
  order     Thermoplasmatales                                       4            563              310       
  order     Thermoproteales                                         13           315              257       
  order     Thermotogales                                           14           460              267       
  order     Thiotrichales                                           64           406              251       
  order     Verrucomicrobiales                                      7            403              284       
  order     Vibrionales                                             80           922              367       
  order     Xanthomonadales                                         63           499              264       
  family    Acetobacteraceae                                        30           473              264       
  family    Acholeplasmataceae                                      12           176               88       
  family    Acidaminococcaceae                                      7            677              256       
  family    Acidilobaceae                                           2            645              144       
  family    Acidithiobacillaceae                                    5            907              200       
  family    Acidobacteriaceae                                       9            575              396       
  family    Aciduliprofundum                                        2            763              130       
  family    Actinomycetaceae                                        42           420              211       
  family    Actinopolysporaceae                                     3            872              433       
  family    Aerococcaceae                                           10           424              195       
  family    Aeromonadaceae                                          17           841              368       
  family    Alcaligenaceae                                          27           486              218       
  family    Alcanivoracaceae                                        8            877              350       
  family    Alicyclobacillaceae                                     9            608              229       
  family    Alteromonadaceae                                        38           590              296       
  family    Anaplasmataceae                                         18           319              212       
  family    Aquificaceae                                            10           656              475       
  family    Archaeoglobaceae                                        5            619              392       
  family    Arthrospira                                             2            1432             419       
  family    Aurantimonadaceae                                       4            883              434       
  family    Bacillaceae                                            162           418              155       
  family    Bacteroidaceae                                          96           524              282       
  family    Bankia                                                  2            1388             332       
  family    Bartonellaceae                                          29           708              188       
  family    Bdellovibrionaceae                                      4            561              331       
  family    Beijerinckiaceae                                        4            1024             488       
  family    Bifidobacteriaceae                                      77           464              220       
  family    Blattabacteriaceae                                      6            547               16       
  family    Brachyspiraceae                                         10           783              458       
  family    Bradyrhizobiaceae                                       52           619              276       
  family    Brevibacteriaceae                                       3            833              259       
  family    Brucellaceae                                            94           1206             241       
  family    Burkholderiaceae                                        94           568              224       
  family    Campylobacteraceae                                      66           505              274       
  family    Cardiobacteriaceae                                      3            807              349       
  family    Carnobacteriaceae                                       7            469              197       
  family    Caulobacteraceae                                        15           644              366       
  family    Cellulomonadaceae                                       7            766              260       
  family    Chitinophagaceae                                        8            634              396       
  family    Chlamydiaceae                                           59           590               92       
  family    Chlorobiaceae                                           12           612              333       
  family    Chloroflexaceae                                         7            570              386       
  family    Chromatiaceae                                           14           545              300       
  family    Chroococcidiopsis                                       2            1159             811       
  family    Clostridiaceae                                          10           387              178       
  family    Clostridiales Family XI. Incertae Sedis                 20           345              186       
  family    Clostridiales Family XVII. Incertae Sedis               5            682              263       
  family    Colwelliaceae                                           3            1371             293       
  family    Comamonadaceae                                          41           669              297       
  family    Conexibacteraceae                                       2            1064             347       
  family    Coriobacteriaceae                                       23           304              155       
  family    Corynebacteriaceae                                      80           585              243       
  family    Coxiellaceae                                            5            1047             146       
  family    Cryomorphaceae                                          3            782              490       
  family    Cyanobacterium                                          4            650              477       
  family    Cyanobium                                               2            1225             277       
  family    Cyanothece                                              8            865              653       
  family    Cyclobacteriaceae                                       13           720              342       
  family    Cytophagaceae                                           30           383              278       
  family    Deferribacteraceae                                      5            768              300       
  family    Dehalococcoidaceae                                      5            755               54       
  family    Deinococcaceae                                          16           630              426       
  family    Dermabacteraceae                                        2            876              242       
  family    Dermacoccaceae                                          3            776              316       
  family    Dermatophilaceae                                        3            866              346       
  family    Desulfobacteraceae                                      14           394              216       
  family    Desulfobulbaceae                                        6            626              290       
  family    Desulfohalobiaceae                                      3            774              358       
  family    Desulfomicrobiaceae                                     2            1255             271       
  family    Desulfovibrionaceae                                     34           497              270       
  family    Desulfurellaceae                                        4            817              252       
  family    Desulfurobacteriaceae                                   2            965              410       
  family    Desulfurococcaceae                                      12           310              223       
  family    Dictyoglomaceae                                         2            1060             103       
  family    Ectothiorhodospiraceae                                  77           837              294       
  family    Enterobacteriaceae                                     262           297              121       
  family    Enterococcaceae                                         55           542              226       
  family    Entomoplasmataceae                                      8            383              120       
  family    Erysipelotrichaceae                                     18           301              159       
  family    Erythrobacteraceae                                      4            1071             266       
  family    Eubacteriaceae                                          22           221              140       
  family    Exiguobacterium                                         11           1039             263       
  family    Fangia                                                  2            1383             144       
  family    Ferrimonadaceae                                         4            1311             393       
  family    Fischerella                                             4            1081             585       
  family    Flammeovirgaceae                                        5            626              437       
  family    Flavobacteriaceae                                      115           457              309       
  family    Francisellaceae                                         40           787              238       
  family    Frankiaceae                                             12           569              282       
  family    Fusobacteriaceae                                        23           431              189       
  family    Gemella                                                 5            787              169       
  family    Geobacteraceae                                          11           631              310       
  family    Geodermatophilaceae                                     5            878              296       
  family    Gloeocapsa                                              2            1115             740       
  family    Glycomycetaceae                                         4            840              375       
  family    Gordoniaceae                                            26           670              298       
  family    Hahellaceae                                             3            1080             468       
  family    Halanaerobiaceae                                        3            702              192       
  family    Halobacteriaceae                                        58           371              244       
  family    Halobacteroidaceae                                      4            788              219       
  family    Halomonadaceae                                          15           684              246       
  family    Helicobacteraceae                                       41           498              302       
  family    Holophagaceae                                           2            883              459       
  family    Hydrogenophilaceae                                      3            1205             209       
  family    Hydrogenothermaceae                                     6            840              402       
  family    Hyphomicrobiaceae                                       12           551              325       
  family    Hyphomonadaceae                                         10           761              398       
  family    Idiomarinaceae                                          5            1057             247       
  family    Intrasporangiaceae                                      8            651              328       
  family    Jonesiaceae                                             2            1211             191       
  family    Lachnospiraceae                                         86           312              160       
  family    Lactobacillaceae                                       143           396              153       
  family    Legionellaceae                                          18           695              290       
  family    Leptolyngbya                                            3            917              660       
  family    Leptospiraceae                                          5            902              533       
  family    Leptotrichiaceae                                        9            467              239       
  family    Leuconostocaceae                                        35           463              209       
  family    Listeriaceae                                            25           873              158       
  family    Lyngbya                                                 2            1111             753       
  family    Mariprofundaceae                                        2            1329             184       
  family    Methanobacteriaceae                                     11           543              255       
  family    Methanocaldococcaceae                                   7            743              524       
  family    Methanocellaceae                                        3            712              317       
  family    Methanococcaceae                                        9            669              370       
  family    Methanocorpusculaceae                                   2            889              135       
  family    Methanomicrobiaceae                                     4            645              231       
  family    Methanoregulaceae                                       4            620              220       
  family    Methanosaetaceae                                        3            700              354       
  family    Methanosarcinaceae                                      10           579              256       
  family    Methylobacteriaceae                                     20           455              308       
  family    Methylococcaceae                                        15           684              348       
  family    Methylocystaceae                                        11           610              373       
  family    Methylophilaceae                                        14           873              269       
  family    Microbacteriaceae                                       30           416              190       
  family    Microchaetaceae                                         2            1208             646       
  family    Micrococcaceae                                          38           459              217       
  family    Microcoleus                                             2            1191             754       
  family    Micromonosporaceae                                      55           713              273       
  family    Moraxellaceae                                           86           689              365       
  family    Mycobacteriaceae                                       101           682              290       
  family    Mycoplasmataceae                                        98           217              127       
  family    Myxococcaceae                                           6            707              364       
  family    Nakamurellaceae                                         2            1011             427       
  family    Nautiliaceae                                            4            712              351       
  family    Neisseriaceae                                           69           658              446       
  family    Nitrosomonadaceae                                       7            851              314       
  family    Nitrosopumilaceae                                       2            714              109       
  family    Nitrospiraceae                                          4            676              379       
  family    Nocardiaceae                                            21           559              269       
  family    Nocardioidaceae                                         11           470              262       
  family    Nocardiopsaceae                                         7            826              316       
  family    Nostocaceae                                             13           800              536       
  family    Oceanospirillaceae                                      14           636              305       
  family    Opitutaceae                                             3            859              521       
  family    Oscillatoria                                            3            1076             673       
  family    Oxalobacteraceae                                        16           580              265       
  family    Paenibacillaceae                                        44           492              188       
  family    Parachlamydiaceae                                       3            775              331       
  family    Pasteurellaceae                                         83           767              440       
  family    Patulibacteraceae                                       2            1012             347       
  family    Pelagibacteraceae                                       15           695              142       
  family    Pelobacteraceae                                         2            958              487       
  family    Peptococcaceae                                          23           391              153       
  family    Peptostreptococcaceae                                  184           287              151       
  family    Phyllobacteriaceae                                      26           598              258       
  family    Piscirickettsiaceae                                     16           638              292       
  family    Planctomycetaceae                                       10           370              265       
  family    Planktothrix                                            6            1307             425       
  family    Planococcaceae                                          6            823              233       
  family    Pleurocapsa                                             2            1138             783       
  family    Porphyromonadaceae                                      37           455              310       
  family    Prevotellaceae                                          69           553              298       
  family    Prochlorococcaceae                                      18           806              240       
  family    Promicromonosporaceae                                   6            783              376       
  family    Propionibacteriaceae                                    40           532              294       
  family    Pseudanabaena                                           2            1172             781       
  family    Pseudoalteromonadaceae                                  21           782              307       
  family    Pseudomonadaceae                                       186           800              302       
  family    Pseudonocardiaceae                                      32           364              201       
  family    Psychromonadaceae                                       5            1062             326       
  family    Pyrodictiaceae                                          2            609              346       
  family    Rhizobiaceae                                            85           515              225       
  family    Rhodobacteraceae                                        91           528              341       
  family    Rhodobiaceae                                            2            1121             424       
  family    Rhodocyclaceae                                          25           641              269       
  family    Rhodospirillaceae                                       26           300              163       
  family    Rhodothermaceae                                         5            801              466       
  family    Rickettsiaceae                                          46           528              196       
  family    Rikenellaceae                                           6            648              386       
  family    Rivulariaceae                                           3            1070             642       
  family    Rubrivivax                                              3            1586             343       
  family    Rubrobacteraceae                                        2            991              269       
  family    Ruminococcaceae                                         20           284              161       
  family    Saprospiraceae                                          5            671              479       
  family    Shewanellaceae                                          26           989              339       
  family    Sinobacteraceae                                         4            1083             398       
  family    Solirubrobacteraceae                                    2            995              398       
  family    Sphingobacteriaceae                                     11           575              344       
  family    Sphingomonadaceae                                       27           501              275       
  family    Spirochaetaceae                                         56           235              124       
  family    Sporolactobacillaceae                                   2            1066             320       
  family    Staphylococcaceae                                       64           642              181       
  family    Streptococcaceae                                       240           524              282       
  family    Streptomycetaceae                                       58           506              264       
  family    Streptosporangiaceae                                    2            1116             473       
  family    Succinivibrionaceae                                     6            637              317       
  family    Sulfolobaceae                                           19           526              149       
  family    Sutterellaceae                                          4            792              261       
  family    Synechococcus                                           25           554              425       
  family    Synechocystis                                           5            1099             794       
  family    Synergistaceae                                          13           452              160       
  family    Syntrophomonadaceae                                     3            744              271       
  family    Teredinibacter                                          10           1162             360       
  family    Thermaceae                                              23           585              331       
  family    Thermoactinomycetaceae                                  2            1027             387       
  family    Thermoanaerobacteraceae                                 24           438              193       
  family    Thermoanaerobacterales Family III. Incertae Sedis       16           599              251       
  family    Thermococcaceae                                         16           500              315       
  family    Thermodesulfobacteriaceae                               5            813              440       
  family    Thermodesulfobiaceae                                    3            538              284       
  family    Thermomonosporaceae                                     4            839              346       
  family    Thermoplasmataceae                                      2            733              167       
  family    Thermoproteaceae                                        12           410              333       
  family    Thermotogaceae                                          14           460              267       
  family    Thiomonas                                               3            1202             366       
  family    Thiotrichaceae                                          6            764              473       
  family    Veillonellaceae                                         57           345              171       
  family    Verrucomicrobiaceae                                     5            505              342       
  family    Vibrionaceae                                            80           922              367       
  family    Xanthobacteraceae                                       9            824              433       
  family    Xanthomonadaceae                                        58           540              311       
  genus     Acetobacter                                             11           1037             392       
  genus     Acholeplasma                                            7            309              151       
  genus     Achromobacter                                           5            1298             427       
  genus     Acidaminococcus                                         5            910              256       
  genus     Acidilobus                                              2            645              144       
  genus     Acidiphilium                                            3            1244             339       
  genus     Acidithiobacillus                                       5            907              200       
  genus     Acidobacterium                                          2            1151             582       
  genus     Acidovorax                                              9            931              335       
  genus     Aciduliprofundum                                        2            763              130       
  genus     Acinetobacter                                           66           883              281       
  genus     Actinobacillus                                          18           1004             589       
  genus     Actinobaculum                                           5            606              222       
  genus     Actinomadura                                            3            940              376       
  genus     Actinomyces                                             27           432              214       
  genus     Actinoplanes                                            3            1041             394       
  genus     Actinopolyspora                                         3            872              433       
  genus     Aequorivita                                             2            1087             283       
  genus     Aerococcus                                              2            719              247       
  genus     Aeromonas                                               12           1380             463       
  genus     Aggregatibacter                                         13           991              357       
  genus     Agrobacterium                                           12           994              379       
  genus     Agromyces                                               2            1096             287       
  genus     Ahrensia                                                2            1213             422       
  genus     Alcaligenes                                             2            1577             235       
  genus     Alcanivorax                                             6            1175             317       
  genus     Algoriphagus                                            5            947              361       
  genus     Aliagarivorans                                          2            1740             251       
  genus     Alicycliphilus                                          2            1650             258       
  genus     Alicyclobacillus                                        8            664              240       
  genus     Alishewanella                                           2            1677             232       
  genus     Alistipes                                               5            703              335       
  genus     Alkaliphilus                                            2            970              326       
  genus     Alloscardovia                                           2            950              116       
  genus     Alteromonas                                             6            1419             314       
  genus     Aminobacterium                                          2            950              145       
  genus     Amycolatopsis                                           12           644              302       
  genus     Anabaena                                                5            992              584       
  genus     Anaerococcus                                            6            665              181       
  genus     Anaeromyxobacter                                        4            1162             375       
  genus     Anaplasma                                               2            688              127       
  genus     Anoxybacillus                                           5            1141             212       
  genus     Aquimarina                                              2            1084             373       
  genus     Archaeoglobus                                           4            662              415       
  genus     Arcobacter                                              8            804              234       
  genus     Arenibacter                                             2            1200             269       
  genus     Arsenophonus                                            4            640              164       
  genus     Arthrobacter                                            25           473              190       
  genus     Arthrospira                                             2            1432             419       
  genus     Asticcacaulis                                           3            1116             409       
  genus     Atopobium                                               8            486              194       
  genus     Aurantimonas                                            2            1288             356       
  genus     Azoarcus                                                2            1311             376       
  genus     Azorhizobium                                            2            1547             390       
  genus     Azospira                                                2            1578             218       
  genus     Azospirillum                                            3            1038             549       
  genus     Azotobacter                                             3            1930             269       
  genus     Bacillus                                               122           491              170       
  genus     Bacteroides                                             96           524              282       
  genus     Bankia                                                  2            1388             332       
  genus     Barnesiella                                             2            1045             230       
  genus     Bartonella                                              29           708              188       
  genus     Bdellovibrio                                            3            843              296       
  genus     Bifidobacterium                                         57           582              204       
  genus     Blastococcus                                            2            1123             317       
  genus     Blattabacterium                                         6            547               16       
  genus     Blautia                                                 7            649              291       
  genus     Bordetella                                              9            892              277       
  genus     Borrelia                                                17           474               66       
  genus     Brachyspira                                             10           783              458       
  genus     Bradyrhizobium                                          36           826              337       
  genus     Brevibacillus                                           4            1162             391       
  genus     Brevibacterium                                          3            833              259       
  genus     Brevundimonas                                           6            952              452       
  genus     Brucella                                                90           1371             236       
  genus     Buchnera                                                10           380               35       
  genus     Burkholderia                                            66           724              235       
  genus     Butyricimonas                                           2            1102             296       
  genus     Butyrivibrio                                            31           578              245       
  genus     Caldicellulosiruptor                                    11           1010             169       
  genus     Calothrix                                               2            1244             726       
  genus     Campylobacter                                           55           598              263       
  genus     Candidatus Arthromitus                                  5            889              153       
  genus     Candidatus Blochmannia                                  3            714               3        
  genus     Candidatus Liberibacter                                 3            764              141       
  genus     Candidatus Pelagibacter                                 12           715              144       
  genus     Candidatus Pelagibacter-like                            3            911               76       
  genus     Candidatus Phytoplasma                                  4            294               86       
  genus     Capnocytophaga                                          15           728              514       
  genus     Carboxydothermus                                        2            1295             139       
  genus     Cardiobacterium                                         2            1153             474       
  genus     Caulobacter                                             4            1219             375       
  genus     Cellulomonas                                            6            805              257       
  genus     Cellulophaga                                            2            1092             357       
  genus     Chitinilyticum                                          2            1527             343       
  genus     Chlamydia                                               43           609               88       
  genus     Chlamydophila                                           16           616               71       
  genus     Chlorobaculum                                           2            1098             186       
  genus     Chlorobium                                              6            820              277       
  genus     Chloroflexus                                            5            659              449       
  genus     Chroococcidiopsis                                       2            1159             811       
  genus     Chryseobacterium                                        6            812              414       
  genus     Citreicella                                             2            1369             390       
  genus     Citrobacter                                             8            1566             347       
  genus     Citromicrobium                                          2            1356             186       
  genus     Clavibacter                                             2            1152             228       
  genus     Clostridium                                            180           308              161       
  genus     Cohnella                                                3            1072             306       
  genus     Collinsella                                             2            829              315       
  genus     Colwellia                                               3            1371             293       
  genus     Comamonas                                               5            1047             399       
  genus     Commensalibacter                                        2            1241             137       
  genus     Conchiformibius                                         2            1237             317       
  genus     Coprobacillus                                           3            833              331       
  genus     Coprococcus                                             3            780              260       
  genus     Coprothermobacter                                       2            870               70       
  genus     Corynebacterium                                         80           585              243       
  genus     Coxiella                                                5            1047             146       
  genus     Cronobacter                                             4            1838             338       
  genus     Cupriavidus                                             8            1163             352       
  genus     Curtobacterium                                          2            952              330       
  genus     Curvibacter                                             2            1707             367       
  genus     Cyanobacterium                                          4            650              477       
  genus     Cyanobium                                               2            1225             277       
  genus     Cyanothece                                              8            865              653       
  genus     Cycloclasticus                                          2            1507              85       
  genus     Cytophaga                                               3            745              464       
  genus     Dechloromonas                                           4            1203             276       
  genus     Dehalobacter                                            3            1160             157       
  genus     Dehalococcoides                                         5            755               54       
  genus     Deinococcus                                             16           630              426       
  genus     Delftia                                                 3            1683             409       
  genus     Desulfatibacillum                                       2            1377             388       
  genus     Desulfobacterium                                        3            680              365       
  genus     Desulfobulbus                                           4            863              276       
  genus     Desulfomicrobium                                        2            1255             271       
  genus     Desulfosporosinus                                       4            915              249       
  genus     Desulfotignum                                           2            1216             274       
  genus     Desulfotomaculum                                        9            640              214       
  genus     Desulfovibrio                                           31           431              224       
  genus     Desulfurococcus                                         4            599              129       
  genus     Dialister                                               4            695              210       
  genus     Dickeya                                                 4            1671             311       
  genus     Dictyoglomus                                            2            1060             103       
  genus     Dorea                                                   4            835              261       
  genus     Dyadobacter                                             4            1056             319       
  genus     Dysgonomonas                                            3            912              480       
  genus     Echinicola                                              2            1196             350       
  genus     Ectothiorhodospira                                      2            1465             274       
  genus     Edwardsiella                                            2            1903             174       
  genus     Eggerthella                                             4            843              301       
  genus     Ehrlichia                                               6            704               87       
  genus     Elizabethkingia                                         3            1251             240       
  genus     Ensifer                                                 26           1043             323       
  genus     Enterobacter                                            13           1338             370       
  genus     Enterococcus                                            51           672              259       
  genus     Enterovibrio                                            2            1819             389       
  genus     Entomoplasma                                            3            460              109       
  genus     Eremococcus                                             2            922              109       
  genus     Erwinia                                                 8            1420             280       
  genus     Erysipelothrix                                          2            843               95       
  genus     Erythrobacter                                           3            1146             256       
  genus     Escherichia                                             27           1212             320       
  genus     Eubacterium                                             18           234              157       
  genus     Exiguobacterium                                         11           1039             263       
  genus     Facklamia                                               5            575              184       
  genus     Faecalibacterium                                        2            1019             264       
  genus     Fangia                                                  2            1383             144       
  genus     Ferrimonas                                              4            1311             393       
  genus     Finegoldia                                              5            878              111       
  genus     Fischerella                                             4            1081             585       
  genus     Flavobacterium                                          20           575              320       
  genus     Flexibacter                                             3            877              663       
  genus     Francisella                                             40           787              238       
  genus     Frankia                                                 11           597              285       
  genus     Fusobacterium                                           21           502              210       
  genus     Gallibacterium                                          2            1519             168       
  genus     Gardnerella                                             14           621              237       
  genus     Gemella                                                 5            787              169       
  genus     Geobacillus                                             15           823              228       
  genus     Geobacter                                               10           732              332       
  genus     Gillisia                                                2            1096             276       
  genus     Glaciecola                                              9            1063             369       
  genus     Gloeocapsa                                              2            1115             740       
  genus     Gluconacetobacter                                       4            1030             240       
  genus     Gluconobacter                                           3            1076             207       
  genus     Glycomyces                                              2            1177             326       
  genus     Gordonia                                                26           670              298       
  genus     Gramella                                                3            1089             213       
  genus     Granulicatella                                          2            891              249       
  genus     Granulicella                                            2            1075             507       
  genus     Haemophilus                                             35           829              480       
  genus     Hahella                                                 2            1567             475       
  genus     Halalkalicoccus                                         2            1003             212       
  genus     Halanaerobium                                           2            898              186       
  genus     Haliea                                                  2            1214             329       
  genus     Haloarcula                                              7            842              250       
  genus     Halobacillus                                            3            1266             292       
  genus     Halobacterium                                           3            704              224       
  genus     Halobiforma                                             2            1026             320       
  genus     Halococcus                                              2            767              347       
  genus     Haloferax                                               10           763              229       
  genus     Halomicrobium                                           2            970              233       
  genus     Halomonas                                               11           840              268       
  genus     Haloquadratum                                           3            798              192       
  genus     Halorubrum                                              4            710              295       
  genus     Haloterrigena                                           2            826              332       
  genus     Helicobacter                                            33           490              276       
  genus     Herbaspirillum                                          3            1324             341       
  genus     Hippea                                                  3            979              157       
  genus     Hirschia                                                2            1333             222       
  genus     Histophilus                                             2            1341              98       
  genus     Hydrogenobacter                                         2            1116              52       
  genus     Hydrogenobaculum                                        5            989               63       
  genus     Hymenobacter                                            3            998              370       
  genus     Hyphomicrobium                                          7            966              312       
  genus     Idiomarina                                              5            1057             247       
  genus     Isoptericola                                            2            1351             168       
  genus     Janibacter                                              2            1001             385       
  genus     Janthinobacterium                                       4            1001             351       
  genus     Jeotgalicoccus                                          2            1124             142       
  genus     Jonesia                                                 2            1211             191       
  genus     Jonquetella                                             2            987               78       
  genus     Kaistia                                                 2            1516             278       
  genus     Kandleria                                               3            1026             154       
  genus     Kangiella                                               2            1480             165       
  genus     Kingella                                                3            1040             614       
  genus     Klebsiella                                              29           1359             348       
  genus     Labrenzia                                               2            1431             424       
  genus     Lachnobacterium                                         2            1164             173       
  genus     Lachnospira                                             3            1045             196       
  genus     Lactobacillus                                          135           409              155       
  genus     Lactococcus                                             14           683              238       
  genus     Laribacter                                              2            1579             150       
  genus     Leeuwenhoekiella                                        2            1172             263       
  genus     Legionella                                              18           695              290       
  genus     Leifsonia                                               2            888              212       
  genus     Leisingera                                              3            1200             365       
  genus     Leptolyngbya                                            3            917              660       
  genus     Leptospira                                              4            1176             280       
  genus     Leptotrichia                                            7            750              335       
  genus     Leucobacter                                             2            948              255       
  genus     Leuconostoc                                             15           611              181       
  genus     Lewinella                                               2            1073             736       
  genus     Listeria                                                24           1137             165       
  genus     Loktanella                                              5            1000             308       
  genus     Luteimonas                                              2            1277             311       
  genus     Lyngbya                                                 2            1111             753       
  genus     Lysinibacillus                                          3            1381             323       
  genus     Lysobacter                                              2            1235             265       
  genus     Magnetospirillum                                        3            1103             429       
  genus     Mannheimia                                              4            1320             245       
  genus     Maribacter                                              3            905              290       
  genus     Marinobacter                                            8            993              273       
  genus     Marinobacterium                                         3            1157             339       
  genus     Marinomonas                                             4            1256             320       
  genus     Mariprofundus                                           2            1329             184       
  genus     Maritimibacter                                          2            1288             421       
  genus     Massilia                                                3            1363             402       
  genus     Megamonas                                               3            1113             288       
  genus     Megasphaera                                             7            696              211       
  genus     Meiothermus                                             7            852              456       
  genus     Mesoplasma                                              5            409              104       
  genus     Mesorhizobium                                           20           767              381       
  genus     Metallosphaera                                          3            690              177       
  genus     Methanobacterium                                        3            797              214       
  genus     Methanobrevibacter                                      5            748              267       
  genus     Methanocaldococcus                                      5            799              537       
  genus     Methanocella                                            3            712              317       
  genus     Methanococcus                                           7            710              377       
  genus     Methanocorpusculum                                      2            889              135       
  genus     Methanolobus                                            2            944              240       
  genus     Methanoplanus                                           2            830              217       
  genus     Methanosaeta                                            3            700              354       
  genus     Methanosarcina                                          3            849              306       
  genus     Methanothermobacter                                     2            965               76       
  genus     Methanothermococcus                                     2            869              366       
  genus     Methanotorris                                           2            891              279       
  genus     Methylobacter                                           2            1294             436       
  genus     Methylobacterium                                        16           606              394       
  genus     Methylococcus                                           2            1613             188       
  genus     Methylocystis                                           3            1172             334       
  genus     Methylomicrobium                                        2            1316             550       
  genus     Methylomonas                                            4            1325             423       
  genus     Methylophaga                                            4            1119             219       
  genus     Methylophilus                                           4            1346             180       
  genus     Methylopila                                             2            1430             482       
  genus     Methylosarcina                                          2            1415             498       
  genus     Methylosinus                                            3            1300             501       
  genus     Methylotenera                                           6            1078             217       
  genus     Methyloversatilis                                       6            1297             270       
  genus     Methylovorus                                            2            1496             132       
  genus     Microbacterium                                          11           623              280       
  genus     Microcoleus                                             2            1191             754       
  genus     Micromonospora                                          5            921              349       
  genus     Microvirga                                              2            1319             353       
  genus     Mobiluncus                                              8            830              222       
  genus     Moorella                                                2            1125             231       
  genus     Moraxella                                               10           915              523       
  genus     Morganella                                              2            1883             217       
  genus     Mycobacterium                                          100           693              300       
  genus     Mycoplasma                                              83           226              133       
  genus     Myroides                                                7            948              335       
  genus     Natrialba                                               3            716              277       
  genus     Natrinema                                               3            796              297       
  genus     Natronobacterium                                        2            1012             271       
  genus     Natronorubrum                                           2            1039             332       
  genus     Neisseria                                               47           887              501       
  genus     Neorickettsia                                           2            678               16       
  genus     Nesterenkonia                                           2            929              286       
  genus     Niabella                                                2            1123             403       
  genus     Nisaea                                                  2            1230             423       
  genus     Nitratireductor                                         3            1196             283       
  genus     Nitratiruptor                                           2            1056             138       
  genus     Nitrobacter                                             3            1233             239       
  genus     Nitrosococcus                                           2            1342             283       
  genus     Nitrosomonas                                            5            957              324       
  genus     Nitrosospira                                            2            1303             242       
  genus     Nocardia                                                6            799              372       
  genus     Nocardioides                                            8            683              299       
  genus     Nocardiopsis                                            4            1007             327       
  genus     Nostoc                                                  5            995              585       
  genus     Novosphingobium                                         5            907              296       
  genus     Oceanicaulis                                            2            1398             175       
  genus     Oceanicola                                              4            998              439       
  genus     Oceanimonas                                             3            1478             267       
  genus     Oceanobacillus                                          2            1474             211       
  genus     Oceanospirillum                                         2            1479             371       
  genus     Ochrobactrum                                            4            1412             260       
  genus     Octadecabacter                                          2            1308             284       
  genus     Odoribacter                                             2            1031             350       
  genus     Oenococcus                                              13           679              136       
  genus     Oligella                                                2            1242             186       
  genus     Oligotropha                                             2            1592             192       
  genus     Olleya                                                  2            1165             249       
  genus     Olsenella                                               2            781              213       
  genus     Oribacterium                                            5            555              211       
  genus     Orientia                                                2            651               94       
  genus     Ornithobacterium                                        2            1172             138       
  genus     Oscillatoria                                            3            1076             673       
  genus     Oxalobacter                                             2            1179             133       
  genus     Paenibacillus                                           33           473              180       
  genus     Pantoea                                                 10           1403             316       
  genus     Parabacteroides                                         6            970              371       
  genus     Parachlamydia                                           2            1064             203       
  genus     Paracoccus                                              8            917              340       
  genus     Paraprevotella                                          2            1113             229       
  genus     Parascardovia                                           2            908               87       
  genus     Pasteurella                                             8            1083             458       
  genus     Patulibacter                                            2            1012             347       
  genus     Pectobacterium                                          5            1705             291       
  genus     Pediococcus                                             7            664              140       
  genus     Pedobacter                                              5            742              322       
  genus     Pelobacter                                              2            958              487       
  genus     Pelodictyon                                             2            1031             227       
  genus     Pelosinus                                               3            1539             333       
  genus     Peptoniphilus                                           7            533              231       
  genus     Peptostreptococcus                                      3            839              240       
  genus     Persephonella                                           3            1036             256       
  genus     Phaeobacter                                             8            1087             310       
  genus     Photobacterium                                          6            1105             326       
  genus     Photorhabdus                                            3            1648             281       
  genus     Planctomyces                                            3            790              552       
  genus     Planktothrix                                            6            1307             425       
  genus     Planococcus                                             2            1321             253       
  genus     Pleomorphomonas                                         2            1410             316       
  genus     Pleurocapsa                                             2            1138             783       
  genus     Polaribacter                                            3            942              301       
  genus     Polaromonas                                             4            1001             336       
  genus     Polynucleobacter                                        3            968              106       
  genus     Pontibacillus                                           2            1303             324       
  genus     Pontibacter                                             2            1139             379       
  genus     Porphyromonas                                           18           569              346       
  genus     Prevotella                                              65           522              281       
  genus     Prochlorococcus                                         18           806              240       
  genus     Promicromonospora                                       2            1247             525       
  genus     Propionibacterium                                       35           632              302       
  genus     Propionimicrobium                                       2            1112             105       
  genus     Proteus                                                 2            1888             231       
  genus     Providencia                                             6            1517             302       
  genus     Pseudanabaena                                           2            1172             781       
  genus     Pseudoalteromonas                                       20           847              396       
  genus     Pseudobutyrivibrio                                      5            873              228       
  genus     Pseudoclavibacter                                       2            863              298       
  genus     Pseudogulbenkiania                                      3            1447             267       
  genus     Pseudomonas                                            182           833              312       
  genus     Pseudonocardia                                          4            796              343       
  genus     Pseudovibrio                                            2            1687             338       
  genus     Pseudoxanthomonas                                       5            1159             369       
  genus     Psychrobacter                                           6            1010             372       
  genus     Psychroflexus                                           2            1118             211       
  genus     Psychromonas                                            5            1062             326       
  genus     Pyrobaculum                                             6            594              217       
  genus     Pyrococcus                                              6            654              266       
  genus     Rahnella                                                2            1981             296       
  genus     Ralstonia                                               14           893              294       
  genus     Rheinheimera                                            4            1282             378       
  genus     Rhizobium                                               40           863              345       
  genus     Rhodanobacter                                           3            1566             246       
  genus     Rhodobacter                                             8            992              372       
  genus     Rhodococcus                                             14           715              302       
  genus     Rhodonellum                                             2            1473             372       
  genus     Rhodopirellula                                          3            1048             661       
  genus     Rhodopseudomonas                                        7            1180             388       
  genus     Rhodospirillum                                          3            1208             514       
  genus     Rhodothermus                                            3            1379             216       
  genus     Rickettsia                                              43           662              110       
  genus     Riemerella                                              6            888              396       
  genus     Roseburia                                               2            975              303       
  genus     Roseiflexus                                             2            1231             441       
  genus     Roseobacter                                             7            850              348       
  genus     Roseomonas                                              2            1576             318       
  genus     Roseovarius                                             3            1134             423       
  genus     Rothia                                                  6            884              217       
  genus     Rubrivivax                                              3            1586             343       
  genus     Rubrobacter                                             2            991              269       
  genus     Ruegeria                                                7            996              358       
  genus     Ruminococcus                                            13           436              242       
  genus     Runella                                                 3            1097             525       
  genus     Saccharibacillus                                        2            1247             331       
  genus     Saccharomonospora                                       8            788              337       
  genus     Salinicoccus                                            2            1179             206       
  genus     Salinimicrobium                                         2            1195             260       
  genus     Salinispora                                             45           1031             284       
  genus     Salmonella                                              21           1529             266       
  genus     Saprospira                                              2            1147             326       
  genus     Sediminibacterium                                       3            992              328       
  genus     Selenomonas                                             21           710              294       
  genus     Serratia                                                13           985              293       
  genus     Shewanella                                              26           989              339       
  genus     Shigella                                                34           1437             348       
  genus     Slackia                                                 4            616              193       
  genus     Solirubrobacter                                         2            995              398       
  genus     Solobacterium                                           2            877              170       
  genus     Sphaerochaeta                                           3            685              216       
  genus     Sphingobacterium                                        2            996              521       
  genus     Sphingobium                                             3            1179             403       
  genus     Sphingomonas                                            11           713              351       
  genus     Sphingopyxis                                            2            1195             382       
  genus     Spirochaeta                                             8            487              211       
  genus     Spirosoma                                               4            993              460       
  genus     Sporosarcina                                            3            999              269       
  genus     Staphylococcus                                          60           773              178       
  genus     Staphylothermus                                         2            676              104       
  genus     Stenotrophomonas                                        10           1276             380       
  genus     Streptococcus                                          226           551              259       
  genus     Streptomyces                                            58           506              264       
  genus     Sulfitobacter                                           4            1140             294       
  genus     Sulfobacillus                                           3            986              278       
  genus     Sulfolobus                                              15           621              152       
  genus     Sulfurihydrogenibium                                    3            994              341       
  genus     Sulfurimonas                                            3            853              245       
  genus     Sulfurospirillum                                        3            929              240       
  genus     Sutterella                                              3            935              232       
  genus     Synechococcus                                           25           554              425       
  genus     Synechocystis                                           5            1099             794       
  genus     Tannerella                                              2            822              441       
  genus     Taylorella                                              4            1069              71       
  genus     Tenacibaculum                                           2            1057             272       
  genus     Teredinibacter                                          10           1162             360       
  genus     Tetragenococcus                                         2            953              183       
  genus     Thalassospira                                           3            1406             326       
  genus     Thauera                                                 6            1175             334       
  genus     Thermacetogenium                                        2            1344             157       
  genus     Thermaerobacter                                         2            1204             181       
  genus     Thermanaerovibrio                                       2            1136             117       
  genus     Thermoanaerobacter                                      13           976              195       
  genus     Thermoanaerobacterium                                   4            1117             147       
  genus     Thermobifida                                            2            1421             225       
  genus     Thermococcus                                            9            552              286       
  genus     Thermocrinis                                            2            941              515       
  genus     Thermocrispum                                           2            1264             288       
  genus     Thermodesulfatator                                      2            1149             333       
  genus     Thermodesulfobacterium                                  3            960              243       
  genus     Thermodesulfovibrio                                     3            999              215       
  genus     Thermogladius                                           2            711               60       
  genus     Thermoplasma                                            2            733              167       
  genus     Thermoproteus                                           3            600              281       
  genus     Thermosipho                                             2            1009             148       
  genus     Thermotoga                                              8            768              325       
  genus     Thermus                                                 14           841              311       
  genus     Thioalkalimicrobium                                     2            1175             103       
  genus     Thioalkalivibrio                                        70           1059             270       
  genus     Thiobacillus                                            3            1205             209       
  genus     Thiomicrospira                                          8            954              243       
  genus     Thiomonas                                               3            1202             366       
  genus     Thiothrix                                               4            1003             417       
  genus     Tolumonas                                               2            1431             200       
  genus     Treponema                                               28           326              173       
  genus     Turicibacter                                            2            1183             236       
  genus     Ureaplasma                                              15           372               51       
  genus     Variovorax                                              7            1114             351       
  genus     Veillonella                                             13           757              219       
  genus     Verrucomicrobium                                        3            697              466       
  genus     Vibrio                                                  70           1084             381       
  genus     Vulcanisaeta                                            2            713              173       
  genus     Weissella                                               6            534              171       
  genus     Wigglesworthia                                          2            679               9        
  genus     Wohlfahrtiimonas                                        2            1335              78       
  genus     Wolbachia                                               8            427              275       
  genus     Xanthobacter                                            3            1394             374       
  genus     Xanthomonas                                             16           1163             401       
  genus     Xenorhabdus                                             2            1608             267       
  genus     Xylella                                                 10           1207             178       
  genus     Yersinia                                                35           1383             341       
  genus     Zymomonas                                               3            1142             133       
  species   Acetobacter pasteurianus                                9            1384             160       
  species   Achromobacter piechaudii                                2            1606             428       
  species   Achromobacter xylosoxidans                              2            1503             445       
  species   Acidaminococcus intestini                               2            1109             140       
  species   Acidithiobacillus caldus                                2            1196             153       
  species   Acidithiobacillus ferrooxidans                          2            1293             128       
  species   Acidovorax avenae                                       2            1633             327       
  species   Acinetobacter baumanni                                  5            1413             231       
  species   Acinetobacter baumannii                                 20           1018             298       
  species   Acinetobacter baylyi                                    2            1608             195       
  species   Acinetobacter calcoaceticus                             5            1444             252       
  species   Acinetobacter johnsonii                                 2            1439             204       
  species   Acinetobacter junii                                     4            1336             222       
  species   Acinetobacter lwoffii                                   3            1363             221       
  species   Acinetobacter nosocomialis                              2            1542             214       
  species   Acinetobacter radioresistens                            5            1354             199       
  species   Actinobacillus pleuropneumoniae                         13           1332             210       
  species   Actinobaculum schaalii                                  2            951              128       
  species   Actinomyces graevenitzii                                2            1014             144       
  species   Actinomyces naeslundii                                  2            1068             299       
  species   Actinomyces neuii                                       2            1053             132       
  species   Actinomyces odontolyticus                               2            1061             179       
  species   Aeromonas hydrophila                                    3            1847             312       
  species   Aeromonas veronii                                       6            1752             329       
  species   Aggregatibacter actinomycetemcomitans                   10           1164             217       
  species   Agrobacterium tumefaciens                               2            1574             278       
  species   Alicycliphilus denitrificans                            2            1650             258       
  species   Alicyclobacillus acidocaldarius                         2            1329             157       
  species   Alloscardovia omnicolens                                2            950              116       
  species   Alteromonas macleodii                                   4            1539             274       
  species   Amycolatopsis mediterranei                              3            1557             526       
  species   Anabaena circinalis                                     2            1357             362       
  species   Anaerococcus prevotii                                   2            900              143       
  species   Anaeromyxobacter dehalogenans                           2            1441             335       
  species   Anoxybacillus flavithermus                              2            1366             163       
  species   Archaeoglobus fulgidus                                  2            975              111       
  species   Arcobacter butzleri                                     4            1067             144       
  species   Arthrobacter nicotinovorans                             2            1382             287       
  species   Arthrospira platensis                                   2            1432             419       
  species   Atopobium vaginae                                       2            714              165       
  species   Azotobacter vinelandii                                  3            1930             269       
  species   Bacillus amyloliquefaciens                              9            1568             226       
  species   Bacillus anthracis                                      11           1465             354       
  species   Bacillus cereus                                         20           1114             319       
  species   Bacillus coagulans                                      4            1365             245       
  species   Bacillus licheniformis                                  4            1711             246       
  species   Bacillus megaterium                                     3            1495             292       
  species   Bacillus methanolicus                                   2            1444             206       
  species   Bacillus pumilus                                        2            1707             218       
  species   Bacillus subtilis                                       14           1489             247       
  species   Bacillus thuringiensis                                  11           1205             357       
  species   Bacteroides caccae                                      2            1239             256       
  species   Bacteroides dorei                                       4            1172             271       
  species   Bacteroides eggerthii                                   2            1171             205       
  species   Bacteroides fragilis                                    10           1128             303       
  species   Bacteroides massiliensis                                2            1340             274       
  species   Bacteroides ovatus                                      6            1115             317       
  species   Bacteroides stercoris                                   2            1202             210       
  species   Bacteroides thetaiotaomicron                            6            1177             313       
  species   Bacteroides uniformis                                   2            1214             253       
  species   Bacteroides vulgatus                                    5            1162             307       
  species   Bacteroides xylanisolvens                               12           1025             325       
  species   Bankia setacea                                          2            1388             332       
  species   Bartonella clarridgeiae                                 2            990               52       
  species   Bartonella doshiae                                      2            1022              73       
  species   Bartonella elizabethae                                  3            1021              88       
  species   Bartonella grahamii                                     2            1010              95       
  species   Bartonella quintana                                     2            945               55       
  species   Bartonella tamiae                                       2            1379              94       
  species   Bartonella vinsonii                                     4            964               71       
  species   Bartonella washoensis                                   2            1014              79       
  species   Bdellovibrio bacteriovorus                              2            1015             305       
  species   Bifidobacterium adolescentis                            3            886              116       
  species   Bifidobacterium animalis                                9            871              113       
  species   Bifidobacterium bifidum                                 5            952              140       
  species   Bifidobacterium breve                                   6            871              166       
  species   Bifidobacterium dentium                                 3            991              156       
  species   Bifidobacterium longum                                  15           750              214       
  species   Bifidobacterium pseudolongum                            2            933              123       
  species   Blautia producta                                        2            1444             376       
  species   Blautia wexlerae                                        2            1092             270       
  species   Bordetella holmesii                                     2            1523             215       
  species   Bordetella pertussis                                    2            1599             225       
  species   Borrelia afzelii                                        2            598               23       
  species   Borrelia burgdorferi                                    7            567               36       
  species   Borrelia garinii                                        3            596               51       
  species   Brachyspira hyodysenteriae                              2            1143             201       
  species   Brachyspira pilosicoli                                  4            951              213       
  species   Bradyrhizobium elkanii                                  6            1117             415       
  species   Bradyrhizobium japonicum                                9            1153             389       
  species   Brevibacillus laterosporus                              2            1538             311       
  species   Brucella abortus                                        20           1513             206       
  species   Brucella canis                                          7            1561             163       
  species   Brucella ceti                                           4            1586             299       
  species   Brucella melitensis                                     20           1435             270       
  species   Brucella ovis                                           9            1599             181       
  species   Brucella pinnipedialis                                  2            1625             189       
  species   Brucella suis                                           16           1524             276       
  species   Buchnera aphidicola                                     10           380               35       
  species   Burkholderia cenocepacia                                5            1585             398       
  species   Burkholderia cepacia                                    5            1353             330       
  species   Burkholderia mallei                                     9            1503             397       
  species   Burkholderia mimosarum                                  2            1813             442       
  species   Burkholderia multivorans                                4            1688             361       
  species   Burkholderia pseudomallei                               12           1626             455       
  species   Butyrivibrio fibrisolvens                               4            1083             302       
  species   Butyrivibrio proteoclasticus                            2            1016             327       
  species   Caldicellulosiruptor kristjanssonii                     3            1326             159       
  species   Campylobacter coli                                      20           955              188       
  species   Campylobacter curvus                                    2            1090              92       
  species   Campylobacter fetus                                     2            1143              80       
  species   Campylobacter jejuni                                    20           903              161       
  species   Campylobacter showae                                    2            1098             118       
  species   Campylobacter upsaliensis                               3            1032             103       
  species   Candidatus Pelagibacter ubique                          10           779              128       
  species   Candidatus Pelagibacter-like SAR11                      3            911               76       
  species   Capnocytophaga ochracea                                 3            1077             205       
  species   Caulobacter crescentus                                  2            1670             221       
  species   Cellulomonas flavigena                                  2            1110             325       
  species   Chlamydia muridarum                                     3            683               35       
  species   Chlamydia psittaci                                      20           613               74       
  species   Chlamydia trachomatis                                   20           641               41       
  species   Chlamydophila pneumoniae                                5            683               57       
  species   Chlamydophila psittaci                                  7            706               40       
  species   Chlorobium phaeobacteroides                             2            1081             255       
  species   Citrobacter freundii                                    3            1937             327       
  species   Clavibacter michiganensis                               2            1152             228       
  species   Clostridium acetobutylicum                              3            1498             242       
  species   Clostridium beijerinckii                                2            1312             318       
  species   Clostridium bolteae                                     5            1147             316       
  species   Clostridium botulinum                                   15           653              197       
  species   Clostridium butyricum                                   3            1300             285       
  species   Clostridium clariflavum                                 2            1344             273       
  species   Clostridium clostridioforme                             7            995              304       
  species   Clostridium difficile                                   20           1098             329       
  species   Clostridium glycolicum                                  2            1385             238       
  species   Clostridium kluyveri                                    2            1377             212       
  species   Clostridium pasteurianum                                2            1184             276       
  species   Clostridium perfringens                                 3            1152             185       
  species   Clostridium sporogenes                                  2            1302             242       
  species   Clostridium symbiosum                                   2            1222             322       
  species   Clostridium thermocellum                                5            1325             239       
  species   Comamonas testosteroni                                  3            1563             349       
  species   Corynebacterium accolens                                2            1192             206       
  species   Corynebacterium aurimucosum                             2            1240             186       
  species   Corynebacterium diphtheriae                             15           1041             134       
  species   Corynebacterium efficiens                               2            1320             210       
  species   Corynebacterium glucuronolyticum                        3            1217             176       
  species   Corynebacterium glutamicum                              6            1184             228       
  species   Corynebacterium halotolerans                            2            1353             183       
  species   Corynebacterium jeikeium                                2            1143             157       
  species   Corynebacterium matruchotii                             2            1191             181       
  species   Corynebacterium pseudotuberculosis                      14           1001             123       
  species   Corynebacterium ulcerans                                3            1221             112       
  species   Corynebacterium urealyticum                             2            1146             117       
  species   Coxiella burnetii                                       5            1047             146       
  species   Cupriavidus metallidurans                               2            1706             419       
  species   Cupriavidus necator                                     2            1517             379       
  species   Cupriavidus taiwanensis                                 3            1681             365       
  species   Dechloromonas agitata                                   2            1533             220       
  species   Dehalococcoides mccartyi                                4            803               50       
  species   Deinococcus radiodurans                                 2            1383             206       
  species   Delftia acidovorans                                     2            1757             421       
  species   Desulfovibrio africanus                                 3            1285             284       
  species   Desulfovibrio alaskensis                                2            1327             223       
  species   Desulfovibrio desulfuricans                             2            941              423       
  species   Desulfovibrio vulgaris                                  3            1187             344       
  species   Dickeya dadantii                                        3            1728             307       
  species   Dorea longicatena                                       2            1005             208       
  species   Edwardsiella tarda                                      2            1903             174       
  species   Ehrlichia ruminantium                                   3            760               58       
  species   Elizabethkingia anophelis                               3            1251             240       
  species   Ensifer fredii                                          2            1548             357       
  species   Ensifer medicae                                         5            1601             343       
  species   Ensifer meliloti                                        15           1320             381       
  species   Enterobacter aerogenes                                  2            2126             298       
  species   Enterobacter cloacae                                    6            1787             306       
  species   Enterococcus casseliflavus                              4            1174             221       
  species   Enterococcus faecalis                                   20           792              298       
  species   Enterococcus faecium                                    20           885              257       
  species   Enterococcus gallinarum                                 2            1257             191       
  species   Eremococcus coleocola                                   2            922              109       
  species   Erwinia amylovora                                       3            1878             177       
  species   Erwinia pyrifoliae                                      2            1942             184       
  species   Escherichia coli                                        20           1628             307       
  species   Escherichia fergusonii                                  2            2134             277       
  species   Eubacterium cellulosolvens                              2            1078             198       
  species   Eubacterium saburreum                                   2            1186             223       
  species   Exiguobacterium sibiricum                               2            1429             175       
  species   Exiguobacterium undae                                   2            1484             173       
  species   Facklamia hominis                                       2            955              100       
  species   Finegoldia magna                                        5            878              111       
  species   Francisella noatunensis                                 6            904              235       
  species   Francisella noatunensis noatunensis                     2            1070             209       
  species   Francisella novicida                                    4            1052             152       
  species   Francisella philomiragia                                6            1127             132       
  species   Francisella tularensis                                  20           850              176       
  species   Fusobacterium necrophorum                               2            1080             117       
  species   Fusobacterium nucleatum                                 6            884              256       
  species   Gallibacterium anatis                                   2            1519             168       
  species   Gardnerella vaginalis                                   14           621              237       
  species   Gemella haemolysans                                     2            961              126       
  species   Geobacillus debilis                                     2            1380             195       
  species   Geobacter sulfurreducens                                2            1487             199       
  species   Glaciecola agarilytica                                  2            1802             312       
  species   Gluconacetobacter diazotrophicus                        2            1506             190       
  species   Gordonia amicalis                                       2            1448             324       
  species   Gordonia polyisoprenivorans                             3            1452             366       
  species   Haemophilus ducreyi                                     4            1128             128       
  species   Haemophilus haemolyticus                                3            1281             149       
  species   Haemophilus influenzae                                  17           924              157       
  species   Haemophilus parainfluenzae                              5            1176             631       
  species   Haemophilus parasuis                                    2            1385             136       
  species   Halalkalicoccus jeotgali                                2            1003             212       
  species   Haloarcula californiae                                  2            1047             260       
  species   Haloarcula vallismortis                                 2            1075             244       
  species   Halobiforma lacisalsi                                   2            1026             320       
  species   Haloferax denitrificans                                 2            1052             231       
  species   Haloferax mucosum                                       2            1030             193       
  species   Haloferax sulfurifontis                                 2            1054             237       
  species   Haloferax volcanii                                      3            864              230       
  species   Haloquadratum walsbyi                                   3            798              192       
  species   Helicobacter cetorum                                    2            927              162       
  species   Helicobacter pylori                                     20           773              143       
  species   Histophilus somni                                       2            1341              98       
  species   Hydrogenobacter thermophilus                            2            1116              52       
  species   Hyphomicrobium denitrificans                            2            1390             196       
  species   Isoptericola variabilis                                 2            1351             168       
  species   Jonquetella anthropi                                    2            987               78       
  species   Kandleria vitulina                                      3            1026             154       
  species   Klebsiella oxytoca                                      5            1725             377       
  species   Klebsiella pneumoniae                                   20           1598             383       
  species   Lachnobacterium bovis                                   2            1164             173       
  species   Lachnospira multipara                                   3            1045             196       
  species   Lactobacillus acidophilus                               3            800              127       
  species   Lactobacillus amylovorus                                2            865              112       
  species   Lactobacillus brevis                                    4            752              203       
  species   Lactobacillus buchneri                                  5            853              186       
  species   Lactobacillus casei                                     16           772              222       
  species   Lactobacillus crispatus                                 8            762              178       
  species   Lactobacillus delbrueckii                               7            742              159       
  species   Lactobacillus fermentum                                 3            949              129       
  species   Lactobacillus gasseri                                   5            692              131       
  species   Lactobacillus helveticus                                4            731              148       
  species   Lactobacillus iners                                     15           639              125       
  species   Lactobacillus jensenii                                  5            813              106       
  species   Lactobacillus johnsonii                                 4            777              130       
  species   Lactobacillus paracasei                                 3            1022             225       
  species   Lactobacillus reuteri                                   9            829              185       
  species   Lactobacillus rhamnosus                                 10           952              246       
  species   Lactobacillus ruminis                                   2            989              143       
  species   Lactobacillus salivarius                                5            864              162       
  species   Lactococcus garvieae                                    4            927              157       
  species   Lactococcus lactis                                      10           856              190       
  species   Laribacter hongkongensis                                2            1579             150       
  species   Legionella longbeachae                                  2            1434             256       
  species   Legionella pneumophila                                  8            1140             213       
  species   Legionella sainthelensi                                 2            1272             299       
  species   Leptospira meyeri                                       2            1272             278       
  species   Leptotrichia goodfellowii                               2            1086             182       
  species   Leuconostoc citreum                                     4            908              102       
  species   Leuconostoc gelidum                                     2            985              105       
  species   Leuconostoc kimchii                                     2            990               97       
  species   Leuconostoc mesenteroides                               3            870              110       
  species   Listeria monocytogenes                                  20           1262             179       
  species   Loktanella vestfoldensis                                2            1360             189       
  species   Lysinibacillus fusiformis                               2            1610             299       
  species   Mannheimia haemolytica                                  3            1446             156       
  species   Mariprofundus ferrooxydans                              2            1329             184       
  species   Megasphaera elsdenii                                    2            1168             145       
  species   Megasphaera genomosp.                                   2            973              196       
  species   Mesorhizobium ciceri                                    3            1605             386       
  species   Mesorhizobium loti                                      6            1317             375       
  species   Methanobrevibacter smithii                              3            932               82       
  species   Methanococcus maripaludis                               4            961               83       
  species   Methylobacterium extorquens                             4            1402             302       
  species   Methylococcus capsulatus                                2            1613             188       
  species   Methylotenera mobilis                                   2            1353             143       
  species   Methylotenera versatilis                                2            1251             231       
  species   Methyloversatilis universalis                           3            1615             262       
  species   Mobiluncus curtisii                                     4            1009             115       
  species   Mobiluncus mulieris                                     4            980              165       
  species   Moraxella catarrhalis                                   8            1131             113       
  species   Mycobacterium abscessus                                 20           1230             333       
  species   Mycobacterium avium                                     3            1306             324       
  species   Mycobacterium bovis                                     4            1386             231       
  species   Mycobacterium canettii                                  4            1378             260       
  species   Mycobacterium gilvum                                    2            1474             320       
  species   Mycobacterium intracellulare                            3            1375             278       
  species   Mycobacterium leprae                                    2            1053             195       
  species   Mycobacterium marinum                                   2            1415             358       
  species   Mycobacterium massiliense                               11           1291             291       
  species   Mycobacterium rhodesiae                                 2            1202             411       
  species   Mycobacterium smegmatis                                 2            1151             407       
  species   Mycobacterium tuberculosis                              20           1077             383       
  species   Mycoplasma agalactiae                                   2            457               44       
  species   Mycoplasma arginini                                     2            361              170       
  species   Mycoplasma bovis                                        3            447               52       
  species   Mycoplasma canis                                        5            460               37       
  species   Mycoplasma fermentans                                   2            506               49       
  species   Mycoplasma gallisepticum                                12           451               45       
  species   Mycoplasma genitalium                                   2            435               21       
  species   Mycoplasma hominis                                      2            411               42       
  species   Mycoplasma hyopneumoniae                                3            417               47       
  species   Mycoplasma hyorhinis                                    5            427               49       
  species   Mycoplasma leachii                                      2            551               49       
  species   Mycoplasma mycoides                                     4            476               64       
  species   Mycoplasma pneumoniae                                   3            448               33       
  species   Mycoplasma synoviae                                     2            457               39       
  species   Myroides odoratimimus                                   6            1001             318       
  species   Natronobacterium gregoryi                               2            1012             271       
  species   Natronorubrum tibetense                                 2            1039             332       
  species   Neisseria flavescens                                    2            1305             164       
  species   Neisseria gonorrhoeae                                   14           1201             205       
  species   Neisseria meningitidis                                  20           997              216       
  species   Novosphingobium nitrogenifigens                         2            1642             232       
  species   Oceanicaulis alexandrii                                 2            1398             175       
  species   Ochrobactrum intermedium                                2            1521             251       
  species   Oenococcus oeni                                         12           770              112       
  species   Oligotropha carboxidovorans                             2            1592             192       
  species   Orientia tsutsugamushi                                  2            651               94       
  species   Ornithobacterium rhinotracheale                         2            1172             138       
  species   Oxalobacter formigenes                                  2            1179             133       
  species   Paenibacillus alvei                                     3            1429             356       
  species   Paenibacillus mucilaginosus                             2            1584             433       
  species   Pantoea ananatis                                        4            1893             263       
  species   Parabacteroides merdae                                  3            1201             260       
  species   Parachlamydia acanthamoebae                             2            1064             203       
  species   Parascardovia denticolens                               2            908               87       
  species   Pasteurella multocida                                   5            1400             149       
  species   Pectobacterium carotovorum                              2            1949             285       
  species   Pediococcus acidilactici                                4            836              109       
  species   Pediococcus pentosaceus                                 2            936               98       
  species   Pelosinus fermentans                                    2            1545             299       
  species   Peptoniphilus lacrimalis                                2            937               98       
  species   Peptostreptococcus anaerobius                           2            1024             136       
  species   Persephonella lauensis                                  2            1194             108       
  species   Phaeobacter gallaeciensis                               4            1371             329       
  species   Photobacterium profundum                                2            1714             406       
  species   Polynucleobacter necessarius                            3            968              106       
  species   Porphyromonas asaccharolytica                           2            1014             131       
  species   Porphyromonas gingivalis                                8            974              194       
  species   Prevotella amnii                                        2            985              155       
  species   Prevotella bivia                                        2            1077             150       
  species   Prevotella brevis                                       2            1125             185       
  species   Prevotella buccae                                       2            1106             220       
  species   Prevotella dentalis                                     2            1171             188       
  species   Prevotella denticola                                    3            1015             186       
  species   Prevotella intermedia                                   2            1108             149       
  species   Prevotella maculosa                                     2            1057             211       
  species   Prevotella melaninogenica                               2            1039             225       
  species   Prevotella micans                                       2            1048             165       
  species   Prevotella oralis                                       2            1159             165       
  species   Prevotella oris                                         3            1006             202       
  species   Prevotella ruminicola                                   2            1088             224       
  species   Prevotella timonensis                                   2            983              206       
  species   Prevotella veroralis                                    2            986              200       
  species   Prochlorococcus marinus                                 13           862              183       
  species   Propionibacterium acidifaciens                          2            539              376       
  species   Propionibacterium acidipropionici                       2            1277             233       
  species   Propionibacterium acnes                                 20           1053             218       
  species   Propionibacterium avidum                                2            1195             150       
  species   Propionimicrobium lymphophilum                          2            1112             105       
  species   Proteus mirabilis                                       2            1888             231       
  species   Providencia alcalifaciens                               2            1875             218       
  species   Providencia stuartii                                    2            1918             220       
  species   Pseudoalteromonas haloplanktis                          2            1596             348       
  species   Pseudoalteromonas luteoviolacea                         2            1493             389       
  species   Pseudoalteromonas piscicida                             2            1903             366       
  species   Pseudobutyrivibrio ruminis                              3            937              238       
  species   Pseudomonas aeruginosa                                  19           1617             469       
  species   Pseudomonas brassicacearum                              2            1982             393       
  species   Pseudomonas chlororaphis                                2            1904             402       
  species   Pseudomonas fluorescens                                 9            1154             347       
  species   Pseudomonas fulva                                       2            1488             484       
  species   Pseudomonas mendocina                                   3            1635             338       
  species   Pseudomonas monteilii                                   3            1761             372       
  species   Pseudomonas putida                                      13           1246             410       
  species   Pseudomonas stutzeri                                    13           1189             403       
  species   Pseudomonas syringae                                    20           1439             395       
  species   Pseudomonas thermotolerans                              2            1731             219       
  species   Pseudomonas umsongensis                                 2            1945             378       
  species   Pseudomonas viridiflava                                 3            1876             420       
  species   Pseudoxanthomonas suwonensis                            3            1424             280       
  species   Pyrococcus furiosus                                     2            981               99       
  species   Ralstonia pickettii                                     2            1715             287       
  species   Ralstonia solanacearum                                  4            1543             320       
  species   Rhizobium etli                                          2            1571             331       
  species   Rhizobium leguminosarum                                 20           1104             354       
  species   Rhodobacter sphaeroides                                 5            1409             281       
  species   Rhodococcus equi                                        2            1456             325       
  species   Rhodococcus erythropolis                                2            1456             402       
  species   Rhodococcus pyridinivorans                              2            1329             319       
  species   Rhodonellum psychrophilum                               2            1473             372       
  species   Rhodopseudomonas palustris                              7            1180             388       
  species   Rhodospirillum rubrum                                   2            1731             250       
  species   Rhodothermus marinus                                    3            1379             216       
  species   Rickettsia canadensis                                   2            783               35       
  species   Rickettsia massiliae                                    2            794               67       
  species   Rickettsia prowazekii                                   5            753               44       
  species   Rickettsia rickettsii                                   14           737               72       
  species   Rickettsia slovaca                                      2            807               50       
  species   Rickettsia typhi                                        3            786               24       
  species   Riemerella anatipestifer                                5            999              139       
  species   Rothia dentocariosa                                     2            1057             145       
  species   Rothia mucilaginosa                                     3            990              143       
  species   Rubrivivax benzoatilyticus                              2            1846             310       
  species   Ruminococcus albus                                      2            1187             284       
  species   Ruminococcus flavefaciens                               5            783              287       
  species   Ruminococcus gnavus                                     2            1114             199       
  species   Saccharomonospora azurea                                2            1323             314       
  species   Salinispora arenicola                                   20           1171             326       
  species   Salinispora pacifica                                    20           1005             278       
  species   Salinispora tropica                                     5            1307             311       
  species   Salmonella enterica                                     20           1643             283       
  species   Saprospira grandis                                      2            1147             326       
  species   Selenomonas artemidis                                   2            1227             134       
  species   Selenomonas bovis                                       2            1210             191       
  species   Selenomonas ruminantium                                 4            1010             375       
  species   Selenomonas sputigena                                   2            1404             138       
  species   Serratia marcescens                                     2            1727             324       
  species   Serratia plymuthica                                     5            1990             304       
  species   Serratia proteamaculans                                 2            2115             304       
  species   Shewanella baltica                                      8            1677             338       
  species   Shigella boydii                                         6            1605             348       
  species   Shigella dysenteriae                                    4            1643             335       
  species   Shigella flexneri                                       18           1512             333       
  species   Shigella sonnei                                         5            1936             302       
  species   Solobacterium moorei                                    2            877              170       
  species   Sphingomonas melonis                                    3            1562             203       
  species   Sphingomonas phyllosphaerae                             2            1437             248       
  species   Spirochaeta thermophila                                 2            1122             152       
  species   Staphylococcus aureus                                   20           1169             170       
  species   Staphylococcus epidermidis                              20           933              208       
  species   Staphylococcus hominis                                  2            1209             107       
  species   Staphylococcus lugdunensis                              3            1270             131       
  species   Staphylococcus pseudintermedius                         2            1324             134       
  species   Stenotrophomonas maltophilia                            8            1441             396       
  species   Streptococcus agalactiae                                20           807              204       
  species   Streptococcus anginosus                                 5            990              160       
  species   Streptococcus australis                                 2            1197             124       
  species   Streptococcus bovis                                     5            976              140       
  species   Streptococcus dysgalactiae                              5            982              204       
  species   Streptococcus equi                                      4            961              136       
  species   Streptococcus gallolyticus                              3            1151             131       
  species   Streptococcus infantarius                               2            1097             118       
  species   Streptococcus infantis                                  4            971              131       
  species   Streptococcus intermedius                               3            1049             115       
  species   Streptococcus mitis                                     11           743              256       
  species   Streptococcus mutans                                    8            1053             121       
  species   Streptococcus oralis                                    9            871              196       
  species   Streptococcus parasanguinis                             6            999              203       
  species   Streptococcus parauberis                                3            969              126       
  species   Streptococcus pneumoniae                                20           845              160       
  species   Streptococcus pseudopneumoniae                          3            1102             236       
  species   Streptococcus pseudoporcinus                            2            1166             114       
  species   Streptococcus pyogenes                                  20           885              150       
  species   Streptococcus salivarius                                4            1021             130       
  species   Streptococcus sanguinis                                 13           877              335       
  species   Streptococcus suis                                      14           852              161       
  species   Streptococcus thermophilus                              6            971               96       
  species   Streptococcus urinalis                                  2            1107             100       
  species   Streptomyces cattleya                                   3            1460             495       
  species   Sulfobacillus acidophilus                               2            1273             192       
  species   Sulfolobus acidocaldarius                               3            819              115       
  species   Sulfolobus islandicus                                   9            778              149       
  species   Sulfolobus solfataricus                                 2            822              116       
  species   Sutterella wadsworthensis                               3            935              232       
  species   Synechococcus elongatus                                 2            1468             121       
  species   Taylorella asinigenitalis                               2            1125              58       
  species   Taylorella equigenitalis                                2            1154              53       
  species   Teredinibacter turnerae                                 8            1640             312       
  species   Thauera linaloolentis                                   2            1773             294       
  species   Thermacetogenium phaeum                                 2            1344             157       
  species   Thermoanaerobacterium thermosaccharolyticum             2            1317             136       
  species   Thermobifida fusca                                      2            1421             225       
  species   Thermus oshimai                                         2            1196             112       
  species   Thermus scotoductus                                     2            1098             162       
  species   Thermus thermophilus                                    4            1100             123       
  species   Thioalkalivibrio thiocyanoxidans                        2            1159             330       
  species   Thiobacillus denitrificans                              2            1290             203       
  species   Treponema denticola                                     9            954              221       
  species   Treponema pallidum                                      3            717               44       
  species   Treponema vincentii                                     2            946              178       
  species   Ureaplasma parvum                                       5            420               35       
  species   Ureaplasma urealyticum                                  10           386               50       
  species   Variovorax paradoxus                                    5            1314             368       
  species   Veillonella atypica                                     3            1145             136       
  species   Veillonella parvula                                     2            1174              97       
  species   Vibrio alginolyticus                                    5            1283             392       
  species   Vibrio cholerae                                         20           1729             348       
  species   Vibrio fischeri                                         2            1814             227       
  species   Vibrio harveyi                                          3            1674             373       
  species   Vibrio mimicus                                          2            1752             276       
  species   Vibrio parahaemolyticus                                 12           1733             363       
  species   Vibrio splendidus                                       2            1757             310       
  species   Vibrio vulnificus                                       5            1802             386       
  species   Wigglesworthia glossinidia                              2            679               9        
  species   Wohlfahrtiimonas chitiniclastica                        2            1335              78       
  species   Wolbachia endosymbiont of Culex quinquefasciatus        2            747               56       
  species   Xanthomonas axonopodis                                  5            1627             365       
  species   Xanthomonas campestris                                  5            1623             336       
  species   Xanthomonas oryzae                                      3            1627             339       
  species   Xylella fastidiosa                                      10           1207             178       
  species   Yersinia enterocolitica                                 5            1868             316       
  species   Yersinia pestis                                         20           1683             330       
  species   Yersinia pseudotuberculosis                             4            1975             263       
  species   Zymomonas mobilis                                       3            1142             133       
------------------------------------------------------------------------------------------------------------
[2023-02-17 15:08:02] INFO: { Current stage: 0:00:03.571 || Total: 0:00:03.571 }
```
