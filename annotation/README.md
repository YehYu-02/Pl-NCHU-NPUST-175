# annotation
## Repeat region analysis
```bash
/Data/liyh/Software/RepeatModeler-2.0.3/BuildDatabase -name $DATABASE_PREFIX $genome.fasta
/Data/liyh/Software/RepeatModeler-2.0.3/RepeatModeler -database $DATABASE_PREFIX -pa 8 -LTRStruct
/Data/liyh/Software/RepeatMasker/RepeatMasker $genome.fasta -lib $DATABASE_PREFIX -families.fa -e rmblast -xsmall -s -gff -pa 8 -dir $OUT_DIR
```

## TE region analysis
```bash
grep -v Simple $genome.fasta.out|grep -v rRNA|grep -v Low > $TE.out
perl /home/yenmr/project/nanopore_methylation_genome157_3/TE/rmskToBed.pl $genome.fasta.out |grep -v Simple|grep -v rRNA|grep -v Low > $TE.bed
```

## FUNGAP
###### gene predict
```bash
conda activate fungap
/Data/ZYY/175r/fungap/FunGAP/download_sister_orgs.py --taxon "Purpureocillium lilacinum" --email_address $Gmail --num_sisters 1
zcat sister_orgs/*faa.gz > $db.faa
/Data/ZYY/175r/fungap/FunGAP/get_augustus_species.py --genus_name "Purpureocillium" --email_address $Gmail
/Data/ZYY/175r/fungap/FunGAP/fungap.py --genome_assembly $genome.fasta --trans_read_1 SRR8940714_1.fastq --trans_read_2 SRR8940714_2.fastq --augustus_species fusarium_graminearum --sister_proteome $db.faa --num_cores 8 --busco_dataset hypocreales_odb10 -o $OUT_DIR
```

## BUSCO
```bash
conda activate busco
busco -i $protein.fasta-o $OUT_PREFIX -m protein --auto-lineage-euk
```

## Interproscan
```bash
sed 's/*//g' $fungap_out_prot.faa > $cleaned_fungap_out_prot.faa
interproscan.sh -i $cleaned_fungap_out_prot.faa -f tsv -appl Pfam --goterms -pa --iprlookup -b $base_name --tempdir $TEMP_DIR -u ./
```

## PHI-local-blast
```bash
/Data/liyh/Software/ncbi-blast-2.13.0+/bin/makeblastdb -in $PHI.fasta -out $DATABASE -dbtype 'prot'
/Data/liyh/Software/ncbi-blast-2.13.0+/bin/blastp -query $fungap_out_prot.faa -db $DATABASE -out $OUT -evalue 1E-10 -outfmt 6
```

## dbcan
```bash
conda activate dbcan
dbcan_build --cpus 8 --db-dir db --clean
run_dbcan $fungap_out_prot.faa protein --out_dir $OUT_DIR
```

## antismash
```bash
conda activate py39
curl -q https://dl.secondarymetabolites.org/releases/latest/docker-run_antismash-full > ~/bin/run_antismash
chmod a+x ~/bin/run_antismash
run_antismash $genome.fasta $OUT --genefinding-tool prodigal --cb-general --asf –knownclusterblast
```

# Barrnap
```bash
barrnap -q -k mito mitogenome_complete.fasta
barrnap -q -k euk $genome.fasta $OUT
```
