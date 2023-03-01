# Usage of environment 01 'anvio-7.1'

ssh azur -L 8080:localhost:8080

~/scripts/script_pangenomics_workflow_T20.sh 2>&1 | tee log.txt

## Converting FASTA files into anvi’o contigs databases
# Re-formatting your input FASTA
for f in *.fasta; do anvi-script-reformat-fasta $f -o ../01_SIMPLIFY/${f%.*}_simplify.fasta -l 0 --simplify-names --seq-type NT ; done
# Creating an anvi’o contigs database
for f in *_headfix.fasta; do anvi-gen-contigs-database -f $f -o CD_${f%_*}_CD.db; done
# anvi-run-hmms
for f in *_CD.db; do anvi-run-hmms -c $f -T n; done (n: the highest sequence number)
# anvi-run-ncbi-cogs (COG20)
for f in *_CD.db; do anvi-run-ncbi-cogs -c $f -T 20; done
# Run KOfam HMMs
for f in *_CD.db; do anvi-run-kegg-kofams -c $f -T 20; done

## Generate an anvio genomes storage using
anvi-gen-genomes-storage -e external_database_path.txt -o GDB_XXXX_GENOMES.db
		# external_database_path.txt
		    name	contigs_db_path
		
## Run pangenome analysis using 
		--use-ncbi-blast (default: False) This program uses DIAMOND by default
		--enforce-hierarchical-clustering (default: False)/--skip-hierarchical-clustering (default: False)	
(fast)
anvi-pan-genome -g GDB_XXXX_GENOMES.db -n PROJECT_XXXX -T 20 --enforce-hierarchical-clustering
(slow)
anvi-pan-genome -g GDB_XXXX_GENOMES.db -n PROJECT_XXXX -T 20 --enforce-hierarchical-clustering --use-ncbi-blast 

## defualt summary
	# check collections
 anvi-summarize  -p PROJECT_XXXX/PROJECT_XXXX-PAN.db -g G_XXXX_GENOMES.db --list-collections
 	# add a 'DEFAULT' collection for pangenome
 anvi-script-add-default-collection  -p PROJECT_XXXX/PROJECT_XXXX-PAN.db 
 	# summarize the 'DEFAULT' collection
 anvi-summarize -p PROJECT_XXXX/PROJECT_XXXX-PAN.db -g G_XXXX_GENOMES.db  -C DEFAULT -o SUMMARY-XXXX-default

## Display the results using 
anvi-display-pan -p PROJECT_XXXX/PROJECT_XXXX-PAN.db -g G_XXXX_GENOMES.db --server-only -P xxx

##  ANI calculation
anvi-compute-genome-similarity --program pyANI -i txt-internal-genomes.txt -p PROJECT-Sulfitobacter-PAN/PROJECT-Sulfitobacter-PAN-PAN.db -o pyANI-ANIb -T 20
