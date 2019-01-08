#  Call orthologues and create phylogenetic tree


### Collect data
```
#/work/GIF/remkv6/USDA/17_OrthofinderTreeSecretome/01_GFFandProteins

#Collect protein fastas and gffs for each plant parasitic nematode

#From wormbase
bursaphelenchus_xylophilus.PRJEA64437.WBPS10.protein.fa
ditylenchus_destructor.PRJNA312427.WBPS10.protein.fa
globodera_pallida.PRJEB123.WBPS10.protein.fa
globodera_rostochiensis.PRJEB13504.WBPS10.protein.fa
meloidogyne_floridensis.PRJEB6016.WBPS10.protein.fa
meloidogyne_hapla.PRJNA29083.WBPS10.protein.fa
meloidogyne_incognita.PRJEA28837.WBPS10.protein.fa
bursaphelenchus_xylophilus.PRJEA64437.WBPS11.annotations.gff3
meloidogyne_javanica.PRJEB8714.WBPS11.annotations.gff3
meloidogyne_javanica.PRJEB8714.WBPS11.protein.fa
meloidogyne_incognita.PRJEB8714.WBPS11.annotations.gff3
meloidogyne_hapla.PRJNA29083.WBPS11.annotations.gff3
meloidogyne_floridensis.PRJEB6016.WBPS11.annotations.gff3
meloidogyne_arenaria.PRJEB8714.WBPS11.annotations.gff3
meloidogyne_arenaria.PRJEB8714.WBPS11.protein.fa
globodera_rostochiensis.PRJEB13504.WBPS11.annotations.gff3
globodera_pallida.PRJEB123.WBPS11.annotations.gff3
ditylenchus_destructor.PRJNA312427.WBPS11.annotations.gff3

#This is one I created from G. ellingtonae genome using RNAseq with braker
cp /work/GIF/remkv6/Baum/CamTechGenomeComparison/26_GloboderaSynteny/otherGloboderaGenomes/ellingtonae/braker/GCA_001723225.1_ASM172322v1_genomic/augustus.gff3 G.ellingtonae.gff3
cp /work/GIF/remkv6/Baum/CamTechGenomeComparison/26_GloboderaSynteny/otherGloboderaGenomes/ellingtonae/braker/GCA_001723225.1_ASM172322v1_genomic/augustus.aa G.ellingtonae.protein.fa

Published in BIOXIV at this time.
cp /work/GIF/remkv6/Baum/CamTechGenomeComparison/58_Renamatorium/1_genomeNgff/augustus.aa H.glycines.protein.fa
cp /work/GIF/remkv6/Baum/CamTechGenomeComparison/58_Renamatorium/1_genomeNgff/fixed.augustus.gff3 H.glycines.gff3
```

### Rename genes and gff so that they are reasonably easy to work with
```
#had to make modifications to names of proteins to get the gff and protein.fa to agree
sed 's/\.t/\./g' H.glycines.protein.fa |sed 's/Hetgly\.G/Hetgly\.T/g'  > H.glycinesFixed.protein.fa

module load maker
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Hgly --justify 8 H.glycines.gff3 >H.glycines.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids H.glycines.map H.glycinesFixed.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids H.glycines.map H.glycines.gff3

##Run this for each gff and protein fasta

#B.xylophilus
sed 's/transcript://g' B.xylophilus.gff3 |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' >B.xylophilusFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Bxyl. --justify 8 B.xylophilusFixed.gff3 >B.xylophilus.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids B.xylophilus.map B.xylophilus.protein.fa

#D. destructor
sed 's/transcript://g' D.destructor.gff3 |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' >D.destructorFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Ddes. --justify 8 D.destructorFixed.gff3 >D.destructor.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids D.destructor.map D.destructor.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids D.destructor.map D.destructorFixed.gff3

#G. pallida
sed 's/transcript://g' G.pallida.gff3 |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' >G.pallidaFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Gpal. --justify 8 G.pallidaFixed.gff3 >G.pallida.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids G.pallida.map G.pallida.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids G.pallida.map G.pallidaFixed.gff3

#G. rostochiensis
sed 's/transcript://g' G.rostochiensis.gff3 |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' >G.rostochiensisFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Gros. --justify 8 G.rostochiensisFixed.gff3 >G.rostochiensis.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids G.rostochiensis.map G.rostochiensis.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids G.rostochiensis.map G.rostochiensis.gff3

#G.ellingtonae
cp /work/GIF/remkv6/Baum/CamTechGenomeComparison/26_GloboderaSynteny/otherGloboderaGenomes/ellingtonae/braker/GCA_001723225.1_ASM172322v1_genomic/augustus.gff3 G.ellingtonae.gff3
cp /work/GIF/remkv6/Baum/CamTechGenomeComparison/26_GloboderaSynteny/otherGloboderaGenomes/ellingtonae/braker/GCA_001723225.1_ASM172322v1_genomic/augustus.aa G.ellingtonae.protein.fa


less G.ellingtonae.gff3 |sed 's/\.exon.*;/;/g' |sed 's/\.gene.*;/;/g' |sed 's/\.CDS.*;/;/g' | sed 's/\.transcript.*;/;/g' > G.ellingtonaeFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Gell. --justify 8 G.ellingtonaeFixed.gff3 > G.ellingtonae.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids G.ellingtonae.map G.ellingtonae.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids G.ellingtonae.map G.ellingtonaeFixed.gff3

#M. arenaria
sed 's/transcript://g' M.arenaria.gff3 |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' >M.arenariaFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Mare --justify 8 M.arenariaFixed.gff3 >M.arenaria.map

/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids  M.arenaria.map M.arenariaFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids  M.arenaria.map M.arenaria.protein.fa

#M. hapla
sed 's/transcript://g' M.hapla.gff3 |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' >M.haplaFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Mhap --justify 8 M.haplaFixed.gff3 >M.hapla.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids  M.hapla.map M.hapla.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids  M.hapla.map M.haplaFixed.gff3

#M.floridensis
less M.floridensis.gff3 |sed 's/transcript://g' |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' > M.floridensisFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Mflo --justify 8 M.floridensisFixed.gff3 >M.floridensis.map

/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids M.floridensis.map M.floridensis.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids M.floridensis.map M.floridensisFixed.gff3

#M.incognita
less M.incognita.gff3 |sed 's/transcript://g' |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' > M.incognitaFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Minc --justify 8 M.incognitaFixed.gff3 >M.incognita.map

##The M.incognita gff had scaffold names attached to gene names, so I had to make a separate map.
less M.incognita.map |sed 's/g/\tMinc/g' |cut -f 2,3 >M.incognitaFasta.map

#While this was successful for primary transcripts, it did not change the alternative isoforms.  
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids M.incognitaFasta.map M.incognita.protein.fa
##removing alternative isoforms that were left in the protein fasta file
module load cdbfasta
cdbfasta M.incognita.protein.fa
#only getting primary isoforms, all of which end with "RA"
grep ">" M.incognita.protein.fa |grep "RA" |sed 's/>//g' |cdbyank M.incognita.protein.fa.cidx >M.incognitaFixed.protein.fa

##This map and gff worked fine without  modification to the map, but still had cds, exon, mrna, and gene attached to gene names
less M.incognita.gff3 |sed 's/transcript://g' |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' > M.incognitaFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids M.incognita.map M.incognitaFixed.gff3

# M.javanica
less M.javanica.gff3  |sed 's/transcript://g' |sed 's/exon://g' |sed 's/gene://g' |sed 's/cds://g' > M.javanicaFixed.gff3
/shared/software/GIF/programs/maker/2.31.8b/bin/maker_map_ids --prefix Mjav --justify 8 M.javanicaFixed.gff3 >M.javanica.map
/shared/software/GIF/programs/maker/2.31.8b/bin/map_fasta_ids  M.javanica.map M.javanica.protein.fa
/shared/software/GIF/programs/maker/2.31.8b/bin/map_gff_ids  M.javanica.map M.javanicaFixed.gff3
```

### Gather primary protein isoforms and run orthofinder
```
#/work/GIF/remkv6/USDA/17_OrthofinderTreeSecretome/02_orthofinder
for f in ../01_GFFandProteins/*protein.fa; do ln -s $f; done

#this one has the original names, so we dont want it
unlink M.incognita.protein.fa
unlink H.glycines.protein.fa

#gettting ready to extract only primary isoforms
module load cdbfasta
for f in *fa; do cdbfasta $f;done

#grab the fasta ids, RA is on every primary isoform, get rid of ">", because cdbyank needs it removed
for f in *fa; do grep ">" $f |grep "RA" |sed 's/>//g' |cdbyank $f.cidx >Primary$f; done

#move all primary proteins to a new folder and run orthofinder
#just a note, the reason we want primary is that orthofinder will call alternative spliced isoforms as duplicated, and so flaw the analysis
mkdir 01_orthofinderRun
cd 01_orthofinderRun/
for f in ../Primary*fa; do ln -s $f;done
printf "module load orthofinder\northofinder -t 16 -a 16  -f 01_orthofinderRun/\n" >orthofinder.sh
################################################################
module load orthofinder
orthofinder -t 16 -a 16 -f 01_orthofinderRun/
################################################################
```


## Trying to figure out if I can use annotations from all species for genes that are orthologous to scn genes.
```
#/work/GIF/remkv6/USDA/17_OrthofinderTreeSecretome/02_orthofinder/01_orthofinderRun/Results_Sep06_1/testAnnotate

#All.lists definition
Essentially this is a tabular list of gene\tfunction.  Each gff3 functional annotation needed to be done separately because of gene/scaffold naming and formatting differences.  These files were then concatentated to look something like this.

#All.lists
################################################################################
Bxyl.00000001-RA        Zinc finger C2H2-type
Bxyl.00000002-RA        Structure-specific recognition protein
Bxyl.00000003-RA        BTB/POZ domain
Bxyl.00000004-RA
Bxyl.00000005-RA        Domain of unknown function DB
Bxyl.00000006-RA        IQ motif%2C EF-hand binding site
Bxyl.00000007-RA
Bxyl.00000008-RA        Sirtuin family
Bxyl.00000009-RA        Domain of unknown function DUF19
Bxyl.00000010-RA        Platelet-activating factor acetylhydrolase-like
Bxyl.00000011-RA        Histidine phosphatase superfamily%2C clade-2
Bxyl.00000012-RA        tRNA (guanine-N1-)-methyltransferase%2C eukaryotic
Bxyl.00000013-RA        Histone deacetylase family
Bxyl.00000014-RA        Leucine-rich repeat
Bxyl.00000015-RA
Bxyl.00000016-RA        Lipid droplet-associated hydrolase
Bxyl.00000017-RA        JAB1/MPN/MOV34 metalloenzyme domain
###############################################################################


#separates each set of annotations into a single file named by the H.glycines gene name
###H.glycines.map is just a list of the genes in the H. glycines genome,
less  ../../../../01_GFFandProteins/H.glycines.map|awk '{print $2}' |grep -v "R"|while read line; do grep "$line" ../Orthogroups.txt |tr " " "\n" |grep -f - ../../../../05_GO_Lists/All.lists|awk '{if(NF<2) {print $0"\tNo Annotation"}else {print $0}}' |cut -f 2- |sort|uniq -c |sort -k1,1nr |tr "\n" "," |sed 's/  //g' >$line; done



#grab all of the files with the gene name and do the below.
#fix the line endings so that I can differentiate empty files from nonempty ones
sed -i 's/$/\n/g' Hgly*

#grabs the gene name, even if the file is blank
wc -l Hgly*|awk  '$1==0 {print $2}' >blankfiles.txt
#Prints gene name with functions
for f in Hgly*; do awk '{print FILENAME,$0}' $f;done >nonblankfiles.txt

#is it valid with the proper number of genes? Yes
cat nonblankfiles.txt blankfiles.txt |sort -k1,1V |wc
  29769  849499 8048127
cat nonblankfiles.txt blankfiles.txt |sort -k1,1V |sed 's/,$//g' >AllAnnotations.list

#AllAnnotations.list  truncated for display purposes
################################################################################
Hetgly.G000000001       9 No Annotation,8 Vps54-likeVacuolar protein sorting-associated protein 54%2C C-terminal,5 Vacuolar protein sorting-associated protein 54%2C N-terminal,1 GNAT domain,1 Vps54-likeVacuolar protein sorting-associated protein 54%2C C-terminal %0Amethod:InterPro accession:IPR019515
Hetgly.G000000002        61 Epithelial sodium channel,5 No Annotation
Hetgly.G000000003       8 N-acetyltransferase ESCO%2C zinc-finger,3 Acyl-CoA N-acyltransferase,2 N-acetyltransferase ESCO%2C acetyl-transferase domain,1 N-acetyltransferase ESCO%2C zinc-finger %0Amethod:InterPro accession:IPR028009 ,1 No Annotation
Hetgly.G000000004        12 SWEET sugar transporter
Hetgly.G000000005        10 Cwf19-like protein%2C C-terminal domain-2,5 Cwf19-like%2C C-terminal domain-1,2 No Annotation,1 Cwf19-like protein%2C C-terminal domain-2 %0Amethod:InterPro accession:IPR006768
Hetgly.G000000006        12 G-patch domain,2 Domain of unknown function DUF4187,2 No Annotation,1 G-patch domain %0Amethod:InterPro accession:IPR025239
Hetgly.G000000007        11 Protein of unknown function DUF1712%2C fungi,6 No Annotation,2 G-patch domain
Hetgly.G000000008        11 Transcription factor CBF/NF-Y/archaeal histone domain,1 No Annotation,1 Transcription factor CBF/NF-Y/archaeal histone domain %0Amethod:InterPro accession:IPR009072
Hetgly.G000000009        12 Dpy-30 motif,1 Dpy-30 motif %0Amethod:InterPro accession:IPR037856 ,1 No Annotation
Hetgly.G000000010        50 No Annotation, 35 Helitron helicase-like domain, 27 DNA helicase Pif1-like, 23 DNA helicase,8 DNA helicase %0Amethod:InterPro accession:IPR010285 ,4 P-loop containing nucleoside triphosphate hydrolase,2 DNA helicase Pif1-like %0Amethod:InterPro accession:IPR025476 ,2 DNA helicase Pif1-like %0Amethod:InterPro accession:IPR027417
Hetgly.G000000011       126 DNA helicase Pif1-like,125 No Annotation, 98 Ulp1 protease family%2C C-terminal catalytic domain, 60 DNA helicase, 50 Helitron helicase-like domain, 13 Papain-like cysteine peptidase superfamily, 11 P-loop containing nucleoside triphosphate hydrolase,2 DNA helicase %0Amethod:InterPro accession:IPR010285 ,2 DNA helicase Pif1-like %0Amethod:InterPro accession:IPR025476 ,1 B-box-type zinc finger,1 Helitron helicase-like domain %0Amethod:InterPro accession:IPR038765 ,1 HMG-I/HMG-Y%2C DNA-binding%2C conserved site,1 Kazal domain,1 Myosin head%2C motor domain,1 Ulp1 protease family%2C C-terminal catalytic domain %0Amethod:InterPro accession:IPR038765
Hetgly.G000000012        13 Dehydrogenase%2C E1 component,4 Transketolase-like%2C pyrimidine-binding domain,3 2-oxoglutarate dehydrogenase E1 component,1 Dehydrogenase%2C E1 component %0Amethod:InterPro accession:IPR005475
Hetgly.G000000013        13 Translation initiation factor IF2/IF5,3 W2 domain,1 Translation initiation factor IF2/IF5 %0Amethod:InterPro accession:IPR003307
Hetgly.G000000014       2 No Annotation
################################################################################

#Attempting to integrate the annotations into the gff for scnbase upload display.  
less /work/GIF/remkv6/Baum/CamTechGenomeComparison/58_Renamatorium/12_functional/fixed.augustus_swissprot_iprscan.gff3 |awk '$3=="gene"' |sort -k9,9V  |sed 's/Note=Protein of unknown function;//g' |sed 's/Note.*;//g' |paste - <(cut -f 2- AllAnnotations.list |sed 's/ ,/,/g'| sed 's/, /,/g'|sed 's/  / /g'  |awk '{if(substr($0,1,1)==" ") {print substr($0,2,length($0))} else {print $0}}' |sed 's/ /-/g'| awk '{print "Note="$0}'|sed 's/\t//9' |awk '{if(substr($0,1,11)=="Note=Hetgly") {print " "}else {print $0}}' ) |paste - <(less /work/GIF/remkv6/Baum/CamTechGenomeComparison/58_Renamatorium/12_functional/fixed.augustus_swis
sprot_iprscan.gff3 |awk '$3=="gene"' |sort -k9,9V  |sed 's/Note=Protein of unknown function;//g' |sed 's/Note.*;//g'|sed 's/;/\t/3'|cut -f 10-|awk '{if(substr($1,1,1)=="D") {print ";"$0} else {print $0}}' ) |sed 's/\t//
9' |cat - <(awk '$3!="gene"' /work/GIF/remkv6/Baum/CamTechGenomeComparison/58_Renamatorium/1_genomeNgff/RetryRename/FinalAugustus.gff3 )|sort -k1,1V -k4,5n |sed 's/\t//9'> /work/GIF/remkv6/Baum/CamTechGenomeComparison/5
8_Renamatorium/12_functional/ReworkedFunctionalAugustus.gff3



#fixing problems with gff still containing a fragmented gene
#manually concatenated 17593 with 17594.
making corresponding gene utr, mrna, and peptide sequeences with gff2 fasta.

#this removes name=;
gt gff3 -tidy -retainids  -o fixedaugustus.gff3 augustus.gff3
#gets rid of the hashes that gff2fasta hates
grep -v "#" fixedaugustus.gff3  >FinalAugustus.gff3
#generates the missing pep file that I need
~/common_scripts/gff2fasta.pl genome738sl.polished.mitoFixed.fa FinalAugustus.gff3 FinalAugustus


This essentially works by taking the primary isoforms from each species, (B. xylophilus, D. destructor, G. rostochiensis, G. ellingtonae, G. pallida, M. hapla, M. incognita, M. floridensis, M. arenaria, M. javanica, and H. glycines) and running them through orthofinder to find orthologs.  This assigns each gene with orthology to an orthogroup.  For each H.glycines gene in an orthogroup, the genes for every other species were used to extract annotation information. This information was counted by using exact annotation matches for each gene.  Thus for Hetgly.G000012040, a total of 146 gene annotations were found and sorted by most abundant annotation type --    104-No-Annotation,64-Zinc-finger,-FLYWCH-type,37-MULE-transposase-domain,4-Zinc-finger,-FLYWCH-type- method:InterPro-accession:IPR018289,1-PAZ-domain,1-TSC-22-/-Dip-/-Bun.
This information was added to the gff annotation of the genome using the Note= attribute.  
```

### Identify overlap between interproscan annotations and orthology-based annnotations
```
#/work/GIF/remkv6/Baum/CamTechGenomeComparison/58_Renamatorium/12_functional

#interproscan
less AnnotatedCorrectlyAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g' |wc
  22718   22718  408924

#Orthology annotations

less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g' |wc                                                            22568   22568  406224
#which of these are just "No Annotation"
less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|grep -w  "Note=.-No-Annotation" |sed 's/,/\t/g' |awk 'NF<13' |wc
    2263   27156  245357  
#So how many annotations are added this way?
less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|grep -w  "Note=.-No-Annotation" |sed 's/,/\t/g' |awk 'NF<13' |awk '{print $9}' |sed 's/ID=//g' |cat - <(less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g') |sort |uniq -c |awk '$1!=2{print $2}' |wc
  20305   20305  365490


#How many genes had interproscan annotations only?
less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|grep -w  "Note=.-No-Annotation" |sed 's/,/\t/g' |awk 'NF<13' |awk '{print $9}' |sed 's/ID=//g' |cat - <(less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g') |sort |uniq -c |awk '$1!=2{print $2}' |grep -v -f - <(less AnnotatedCorrectlyAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g' ) |wc
   4990    4990   89820

#How many genes had ortholog-based annotations only?
less AnnotatedCorrectlyAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g' |grep -v -f - <(less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|grep -w  "Note=.-No-Annotation" |sed 's/,/\t/g' |awk 'NF<13' |awk '{print $9}' |sed 's/ID=//g' |cat - <(less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g') |sort |uniq -c |awk '$1!=2{print $2}' ) |wc
   2577    2577   46386

#How many genes total now have annotations? yes, adjusted for "No Annotation" col 9's too

cat <(less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|grep -w  "Note=.-No-Annotation" |sed 's/,/\t/g' |awk 'NF<13' |awk '{print $9}' |sed 's/ID=//g' |cat - <(less ReworkedFunctionalAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g') |sort |uniq -c |awk '$1!=2{print $2}' ) <(less AnnotatedCorrectlyAugustus.gff3 |sed 's/;/\t/g' |awk '$3=="gene"' |awk 'NF>11'|awk '{print $9}' |sed 's/ID=//g') |sort|uniq|wc
  25295   25295  455310


#How many transposase, integrase, or reverse transcriptase genes are there?
grep "ransposase" ReworkedFunctionalAugustus.gff3 |awk '$3=="gene"' |wc
    574    5166  151075
 grep "tegrase" ReworkedFunctionalAugustus.gff3 |awk '$3=="gene"' |wc
   1053    9477  961467
 grep "Reverse-transcriptase" ReworkedFunctionalAugustus.gff3 |awk '$3=="gene"' |wc
    886    7974  880702
 cat <(grep "ransposase" ReworkedFunctionalAugustus.gff3 |awk '$3=="gene"') <(grep "tegrase" ReworkedFunctionalAugustus.gff3 |awk '$3=="gene"' ) <( grep "Reverse-transcriptase" ReworkedFunctionalAugustus.gff3 |awk '$3=="gene"') |sort|uniq|wc
   1853   16677 1178140
```
