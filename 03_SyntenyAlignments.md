# Create synteny tracks for the 5 syntenic Tylenchida species
```
Need to make syntenic tracks to the 5 related species for JBrowse

ln -s ../26_GloboderaSynteny/circos/syntenic.ribbons.txt pallida.syntenic.ribbons.txt
ln -s ../26_GloboderaSynteny/otherGloboderaGenomes/rostochiensis/circos/syntenic.ribbons.txt rostochiensis.syntenic.ribbons.txt
ln -s ../26_GloboderaSynteny/otherGloboderaGenomes/ellingtonae/braker/GCA_001723225.1_ASM172322v1_genomic/circos/syntenic.ribbons.txt ellingtonae.syntenic.ribbons.txt
ln -s ../39_Meloidogyne/hapla/circos/syntenic.ribbons.txt hapla.syntenic.ribbons.txt
ln -s ../39_Meloidogyne/incognita/circos/syntenic.ribbons.txt incognita.syntenic.ribbons.txt

awk  '{print $1,"iadhore","synteny", $2,$3,".","+",".",$4":"$5"-"$6}' ellingtonae.syntenic.ribbons.txt|tr " " "\t" |sort -k 1,1 -k4,4n >ellingtonae.syntenic.ribbons.txt.gff
awk  '{print $1,"iadhore","synteny", $2,$3,".","+",".",$4":"$5"-"$6}' gff.maker.sh|tr " " "\t" |sort -k 1,1 -k4,4n >gff.maker.sh.gff
awk  '{print $1,"iadhore","synteny", $2,$3,".","+",".",$4":"$5"-"$6}' hapla.syntenic.ribbons.txt|tr " " "\t" |sort -k 1,1 -k4,4n >hapla.syntenic.ribbons.txt.gff
awk  '{print $1,"iadhore","synteny", $2,$3,".","+",".",$4":"$5"-"$6}' incognita.syntenic.ribbons.txt|tr " " "\t" |sort -k 1,1 -k4,4n >incognita.syntenic.ribbons.txt.gff
awk  '{print $1,"iadhore","synteny", $2,$3,".","+",".",$4":"$5"-"$6}' pallida.syntenic.ribbons.sorted.txt|tr " " "\t" |sort -k 1,1 -k4,4n >pallida.syntenic.ribbons.sorted.txt.gff
awk  '{print $1,"iadhore","synteny", $2,$3,".","+",".",$4":"$5"-"$6}' rostochiensis.syntenic.ribbons.txt|tr " " "\t" |sort -k 1,1 -k4,4n >rostochiensis.syntenic.ribbons.txt.gff


ls *|while read line; do echo "awk  '{print \$1,\"iadhore\",\"synteny\", \$2,\$3,\".\",\"+\",\".\",\$4\":\"\$5\"-\"\$6}' "$line"|tr \" \" \"\t\" |sort -k 1,1 -k4,4n >"$line".gff" ; done >gff.maker.sh
less gff.maker.sh
```
Index and upload to JBrowse
```
ls *gff|while read line; do bgzip $line; done
ls *gff.gz|while read line; do tabix -p gff $line; done
```
