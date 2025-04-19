# assembly
## basecall, canu assembly and polish
```bash
guppy_basecaller -i $FAST5 -s $OUT_DIR -c dna_r9.4.1_450bps_hac.cfg --num_callers 1 --gpu_runners_per_device 12 --compress_fastq -x 'auto'
cat $OUT_DIR/pass/*.fastq.gz > $basecalling.fastq.gz
porechop -i $basecalling.fastq.gz -o $output.fastq.gz --threads 40
export PATH=/Data/liyh/fungi/143/canu-2.1.1/bin:$PATH
gunzip $output.fastq.gz
paste - - - - < $output.fastq | cut -f 1,2 | sed 's/^@/>/' | tr "\t" "\n" > $reads.fasta
nanopolish index -d $FAST5 $reads.fasta 
canu -p $PREFIX -d $OUT_DIR genomeSize=32m corOutCoverage=40 -nanopore-raw $reads.fasta
bwa index $PREFIX.contigs.fasta
bwa mem -x ont2d -t 8 $PREFIX.contigs.fasta reads.fasta | samtools sort -o $reads.sorted.bam -T $reads.tmp
samtools index $reads.sorted.bam
python3 /Data/liyh/Software/nanopolish/scripts/nanopolish_makerange.py $PREFIX.contigs.fasta | parallel --results $nanopolish.results -P 8 nanopolish variants --consensus -o polished.{1}.vcf -w {1} -r $reads.fasta -b $reads.sorted.bam -g $PREFIX.contigs.fasta -t 4 --min-candidate-frequency 0.1
nanopolish vcf2fasta -g $PREFIX.contigs.fasta polished.*.vcf > $polished.fasta
```

## QUAST
###### Generate report
```bash
pipenv run /Data/liyh/quast-5.1.0rc1/quast.py $polished.fasta --fungus -t 8 -o $polished_output
```

## BUSCO
###### BUSCO analysis to assess the completeness of the genome assembly
```bash
conda activate busco
busco -i $genome.fasta -o $OUT_PREFIX -m genome -l $DATASET --augustus --augustus_species $Augustus_species
```

## Nanoplot
###### Sequencing QC
```bash
conda activate nanoqc
NanoPlot --summary $sequencing_summary.txt --loglength -o $OUTDIR
```
## mitogenome-assembly
```bash
circlator all $assembly.fasta $reads.fasta $OUT_DIR
```
