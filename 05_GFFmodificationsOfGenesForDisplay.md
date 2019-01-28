### the current scnbase gene models do not provide all sequences in the correct orders etc.

This is dependent on the order of the entries in column 3.  I need to sort each one of these for the interpro functional gff.

```
#Desired order
gene
mrna
stop
cds
exon
intron
cds
exon
intron
...
start
```

```
sed 's/Name=;//g' ReworkedFunctionalAugustus_sorted.gff |sed 's/Note=/Name=/g' '>test.gff
git clone https://github.com/billzt/gff3sort.git
perl gff3sort/gff3sort.pl --precise --chr_order natural test5.gff |bgzip >test5.gz
tabix -p gff test5.gz
```
Sent to levi on 01/29/19
