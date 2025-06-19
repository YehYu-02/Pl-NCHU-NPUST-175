# Genome phylogentic-tree
## ufcg
```bash
conda activate ufcg
ufcg profile -i $seq_file -o $out_file
ufcg tree -i $out_file -o $tree_file
```

# Mitogenome phylogentic-tree
## mafft
```bash
mafft --thread 8 --auto $mitogenome.fasta > $mitogenome¬mafft.fasta
```

## partitionfinder
```bash
conda activate py27
python /Data/ZYY/175r/mito/partitionfinder-2.1.1/partitionfinder-2.1.1/PartitionFinder.py ./
```

## RAxML 
###### part.txt from partitionfinder
```bash
./standard-RAxML-8.2.12/raxmlHPC-AVX -s $mito.phy -f a -x 1234 -p 1234 -# 100 -m GTRGAMMA -q $part.txt -n $OUT_PREFIX
```

## IQ-TREE (single substitution)
```bash
conda activate iqtree
iqtree -s $mito.nex -m JTT+G -bb 1000 -nt AUTO
```
