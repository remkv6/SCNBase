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

samtools sort  -@ 16 pJ2_Race_3.bam >sortedpJ2_Race_3.bam
 bedtools genomecov -ibam sortedpJ2_Race_3.bam -bga -g genome738sl.polished.mitoFixed.fa >sortedpJ2_Race_3.bam.bdg
 ~/bedGraphToBigWig sortedpJ2_Race_3.bam.bdg Chr.sizes sortedpJ2_Race_3.bam.bw
