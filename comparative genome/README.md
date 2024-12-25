# Comparative genome
###### Comparative genomics analysis and distinctive genomic regions (DGRs) identify
## mummer
```bash
/Data/liyh/mummer-4.0.0rc1/nucmer --prefix=$PREFIX $Reference_genome $genome.fasta 
/Data/liyh/mummer-4.0.0rc1/show-coords -rcl $PREFIX.delta > $PREFIX.coords
/data/home/ipgenome2/ZYY/Software/seqkit fx2tab $genome.fasta -l -n > $seqkit_out
awk 'OFS="\t"{print $1,0,$2}' $seqkit_out > $seqkit_out.bed
cat $PREFIX.coords | awk 'OFS="\t"{print $18,$1,$2}' |sort -k1 > $PREFIX.position
/data/home/ipgenome2/ZYY/Software/bedtools2/bin/bedtools subtract -a $seqkit_out.bed -b $PREFIX.position > $PREFIX _specific
awk '{print $1,$2,$3,$3 - $2}' $PREFIX _specific > $PREFIX _specific_min.txt
sort -k4 -n -r $PREFIX _specific_min.txt > $PREFIX _specific_min_sort.txt
```

## local blast
```bash
/Data/liyh/Software/ncbi-blast-2.13.0+/bin/makeblastdb -in $Reference_genome.fasta -out $DATABASE -dbtype 'nucl'
/Data/liyh/Software/ncbi-blast-2.13.0+/bin/ blastn -task blastn -query $specific_region -db $DATABASE -outfmt "6 qacc sacc evalue bitscore length qcovs pident" -out $OUT
sort -k6 -n $OUT > $OUT_sort.txt
awk '{if($6<=20) print $0}’ $OUT_sort.txt > $OUT20.txt
awk '{print $1}' $OUT20.txt | uniq > $OUT_uniq.txt
```
