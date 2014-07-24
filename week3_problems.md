Assignment 2: Parse a GFF3 file

One of the things that Perl programs can do is to parse text files and extract the information that your lab is interested in.

Download the GFF3 formatted file for the Mimulus genome. GFF3 format, or gene feature format, is a way to describe features in a genome. Each line of a GFF3 formatted file contains a single feature found in a locus.

GFF3 format is described here:  http://www.sequenceontology.org/gff3.shtml

A typical entry for a gene will look something like this


```
scaffold_1 phytozome9_0 mRNA 4125905 4131904 . - . ID=PAC:17689354;Name=mgv1a000953m;pacid=17689354;longest=1;Parent=mgv1a000953m.g
scaffold_1 phytozome9_0 CDS 4131641 4131904 . - 0 ID=PAC:17689354.CDS.1;Parent=PAC:17689354;pacid=17689354
scaffold_1 phytozome9_0 CDS 4130504 4131139 . - 0 ID=PAC:17689354.CDS.2;Parent=PAC:17689354;pacid=17689354
scaffold_1 phytozome9_0 CDS 4130164 4130407 . - 0 
```

Write a script that takes this GFF3 file and prints out ONLY the lines containing genes and the size of the gene. 

Process this data file to print out the following data:
Average gene size
Average Exon (aka CDS) size
Average three_prime_UTR size
Average five_prime_UTR size
