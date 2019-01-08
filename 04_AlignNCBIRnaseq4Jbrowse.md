# Need to align new rnaseq datasets to the genome for jbrowse
```
#/work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim

#Create list of sra files I want.
################################################################################
SRR5447119
SRR5447118
SRR5447117
SRR5447116
SRR5447115
SRR5447114
SRR5447113
SRR5447112
SRR5447111
SRR5447110
SRR5447109
SRR5447108
################################################################################


#had to use ebi, as ncbi files were not complete or failed to download completely
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/008/SRR5447108/SRR5447108_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/008/SRR5447108/SRR5447108_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/009/SRR5447109/SRR5447109_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/009/SRR5447109/SRR5447109_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/000/SRR5447110/SRR5447110_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/000/SRR5447110/SRR5447110_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/001/SRR5447111/SRR5447111_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/001/SRR5447111/SRR5447111_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/002/SRR5447112/SRR5447112_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/002/SRR5447112/SRR5447112_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/003/SRR5447113/SRR5447113_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/003/SRR5447113/SRR5447113_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/004/SRR5447114/SRR5447114_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/004/SRR5447114/SRR5447114_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/005/SRR5447115/SRR5447115_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/005/SRR5447115/SRR5447115_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/006/SRR5447116/SRR5447116_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/006/SRR5447116/SRR5447116_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/007/SRR5447117/SRR5447117_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/007/SRR5447117/SRR5447117_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/008/SRR5447118/SRR5447118_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/008/SRR5447118/SRR5447118_2.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/009/SRR5447119/SRR5447119_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR544/009/SRR5447119/SRR5447119_2.fastq.gz


```

#Trim, align, sort, convert to bigwig
```
#runFeatureCounts.sh script
###################################################################################################
#runFeatureCountsNBigwig.sh
#note: I premake a hisat2 database beforehand if I want to run this in parallel

#hisat2-build genome.fa genome
#"Your fasta" "a header for your genome, like worm for wormgenome.fa
###############################################################################
#!/bin/bash

#Working model.  Things to consider: the script needs different settings for 2 analyses. The script is set up for na-seq (hisat2), as this is set up for RNA.  free node settings vs regular node settings (procand memory).
#You must provide the following. Note variable DBDIR does not need a "/" at the end.
# sh runFeatureCounts.sh 28 sequence_1.fastq sequence_2.fastq /work/GIF/remkv6/files genome.fa genome.gff3


PROC=16
R1_FQ="$1"
R2_FQ="$2"
DBDIR="$3"
GENOME="$4"
GFF="$5"

module load trimgalore
module load py-setuptools/35.0.2-py2-hqrsjff
trim_galore --paired ${R1_FQ} ${R2_FQ}

module load hisat2
#hisat2-build ${GENOME} ${GENOME%.*}
hisat2 -p ${PROC} -x ${GENOME%.*} -1 ${R1_FQ%%.*}_val_1.fq.gz -2 ${R2_FQ%%.*}_val_2.fq.gz -S ${R1_FQ%.*}.sam

module load samtools
samtools view --threads ${PROC} -b -o ${R1_FQ%.*}.bam ${R1_FQ%.*}.sam
samtools sort -m 7G -o ${R1_FQ%.*}_sorted.bam -T ${R1_FQ%.*}_temp --threads ${PROC} ${R1_FQ%.*}.bam

module load GIF2/java/1.8.0_25-b17
module load picard/2.17.0-ft5qztz
java -jar /opt/rit/spack-app/linux-rhel7-x86_64/gcc-4.8.5/picard-2.17.0-ft5qztzntoymuxiqt3b6yi6uqcmgzmds/bin/picard.jar CollectAlignmentSummaryMetrics  REFERENCE_SEQUENCE=${DBDIR}/${GENOME} INPUT=${R1_FQ%.*}_sorted.bam OUTPUT=${R1_FQ%.*}.bam_alignment.stats

module load bedtools2
bedtools genomecov -ibam ${R1_FQ%.*}_sorted.bam -bga -g ${GENOME} >${R1_FQ%.*}_sorted.bam.bdg
./bedGraphToBigWig ${R1_FQ%.*}_sorted.bam.bdg Chr.sizes ${R1_FQ%.*}_sorted.bw

###################################################################################################


sh runFeatureCounts.sh SRR5447108_1.fastq.gz SRR5447108_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447109_1.fastq.gz SRR5447109_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447110_1.fastq.gz SRR5447110_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447111_1.fastq.gz SRR5447111_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447112_1.fastq.gz SRR5447112_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447113_1.fastq.gz SRR5447113_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447114_1.fastq.gz SRR5447114_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447115_1.fastq.gz SRR5447115_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447116_1.fastq.gz SRR5447116_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447117_1.fastq.gz SRR5447117_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447118_1.fastq.gz SRR5447118_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
sh runFeatureCounts.sh SRR5447119_1.fastq.gz SRR5447119_2.fastq.gz  /work/GIF/remkv6/Baum/05_738NewAnalyses/04_NewRNAseqOnlineAlignment4Jbrowse/01_trim genome738sl.polished.mitoFixed.fa fixed.augustus.gff3
```

Uploaded bigwig files to scnbase
