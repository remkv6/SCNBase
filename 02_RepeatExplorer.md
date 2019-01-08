# Repeat explorer of TN20 H. glycines illumina reads



### RepeatExplorer with 150bp reads, trimmed for quality and adapters, converted to fasta
```
module load parallel
module load trimmomatic
module load fastx-toolkit
module load fastqc

java -jar /shared/software/GIF/programs/trimmomatic/0.36/trimmomatic-0.36.jar PE -threads 16 -phred33 -trimlog SRR1800548_trim.log SRR1800548_1.fastq SRR1800548_2.fastq SRR1800548_1.trimmedpaired.fastq SRR1800548_1.trimmedunpaired.fastq SRR1800548_2.trimmedpaired.fastq SRR1800548_2.trimmedunpaired.fastq ILLUMINACLIP:$TRIMMOMATIC_HOME//adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:20

fastqc SRR1800548_1.trimmedpaired.fastq
fastqc SRR1800548_2.trimmedpaired.fastq

fastq_to_fasta -n -v -i SRR1800548_1.trimmedpaired.fastq -o SRR1800548_1.trimmedpaired.fasta
fastq_to_fasta -n -v -i SRR1800548_2.trimmedpaired.fastq -o SRR1800548_2.trimmedpaired.fasta
```

Interlace the first and second read, filter by read length
```
awk '/^>/{print ">" ++i"r"; next}{print}' SRR1800548_2.trimmedpaired.fasta >SRR1800548_2.trimmedpaired.renamed.fasta
awk '/^>/{print ">" ++i"f"; next}{print}' SRR1800548_1.trimmedpaired.fasta >SRR1800548_1.trimmedpaired.renamed.fasta
module load seqtk
seqtk mergepe SRR1800548_1.trimmedpaired.renamed.fasta SRR1800548_2.trimmedpaired.renamed.fasta >SRR1800548_2.trimmedpaired.interlaced.fasta

module load bioawk
module load cdbfasta
cdbfasta SRR1800548_2.trimmedpaired.interlaced.fasta
bioawk -c fastx '{print $name, length($seq)}' SRR1800548_2.trimmedpaired.interlaced.fasta |awk '$2>149' |sed 's/r/\tr/g'|sed 's/f/\tf/g' |awk '{if ($1==old) print oldline "\n"$0; old=$1;oldline=$0}'|sed 's/\t//1' |awk '{print $1}' |cdbyank SRR1800548_2.trimmedpaired.interlaced.fasta.cidx >ForGalaxyOnline2.fasta
head -n 400000 ForGalaxyOnline2.fasta >ForGalaxyOnline.200k2.fasta
```

### Submitted online at repeatexplorer.org with the following parameters\\
```

Input Parameter	Value	Note for rerun
Input DNA sequences	10: ForGalaxyOnline.200k2.fasta
All sequence reads are paired	True
Rename sequences	True
Length of sample code	0
Minimum overlap length for clustering	55
[%] Cluster size threshold for detailed analysis	0.002
RepeatMasker database	Metazoa
Use custom repeat database	true
Custom library of repeats	4: consensi.fa.classified
Include sequence filtering?	false
Minimal overlap for assembly	50
```


### Sub analysis of Repeat Explorer clusters
Mapping galaxy clusters to the genome with gmap
```
module load gmap-gsnap/20160404
gmap_build -d genome738sl.polished.mitoFixed  -D /data013/GIF/remkv6/Baum/CamTechGenomeComparison/43_RepeatExpClusters/ genome738sl.polished.mitoFixed.fa
gmap -D /data013/GIF/remkv6/Baum/CamTechGenomeComparison/43_RepeatExpClusters/ -d genome738sl.polished.mitoFixed -B 5 -t 16 --input-buffer-size=1000000 --output-buffer-size=1000000 -f 2  Galaxy2.fasta
```


How do the clusters overlap with real repeats in the genome?
```
#Made correctly formatted gff for JBrowse and for a bedtools intersect on known repeats in the genome.
less GMAP-SCN-RepClust.o172605.condo|sed 's/|quiver//g' |sort -k1,1 -k 4,4n |grep -v "###" |grep -v "======" |grep -v "#" >sorted.Galaxy.gff3
awk 'NF==9' sorted.Galaxy.gff3 >sorted.Galaxy.fix.gff3
awk 'NR!=196117' sorted.Galaxy.fix.gff3 |grep -v "everything" >TEST
module load genometools
gt gff3 -sortlines -tidy TEST >repeatexplorer.gff

d
```
