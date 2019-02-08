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
git clone https://github.com/billzt/gff3sort.git
perl gff3sort/gff3sort.pl --precise --chr_order natural ReworkedFunctionalAugustus_sorted.gff |bgzip >OrthologBasedAndIPRScan.gff.gz
module load tabix
tabix -p gff OrthologBasedAndIPRScan.gff.gz

```
Sent to levi on 01/29/19

### Gene name doesnt display properly in JBrowse on some genes like Hetgly.T000000462.1, but it does on the precursor gff files.
```  

#oops, this would have overwrote the previous test5.gff if I unpacked it.  
mv test5.bgz test5.gff.gz
#no gunzip, just to show the path to the file below

mv test5.gff.gz test5-1.gff.gz
mv test5.bgz.tbi test5-1.gff.gz.tbi


 gunzip test5-1.gff.gz

```
