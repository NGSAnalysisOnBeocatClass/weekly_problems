These problems will require that you use what you have learned in class and you may have to work ahead into additional chapters, in particular REGEX to do pattern searching. Use Google+ if you have having problems.

##Problem 1: Parsing fasta files

Write a script that will determine the N50 of a genome assembly.

The N50 is the length of the longest contig such that half the total length of the assembly is composed of contigs equal to or longer than its length.

In other words, process a fasta file and count the lengths of all the lines that contain sequence data. You might want to store these into an array. Then you will need to sort the array from longest to shortest and then process each element of the array until you find the length of the contig that is greater than or equal to half the length of the total assembly.

**Here is the outline or pseudocode of what your script should do:**

1) create and edit a script called ~/scripts/N50.pl

2) Within N50.pl loop through your fasta file and make an array of contig lengths (@lengths) as you read in lines of the fasta. Also calculate the total length of the assembly ($total_length).

3) reverse sort array @lengths so that it goes from longest to shortest

4) create a new variable $fraction and set this equal to $total_length

5) loop through the reverse sorted array subtracting the $current_contig_length from $fraction each time. Do this while $fraction is greater than half the total length.

6) report $current_contig_length when the loop is finished

**Your base script should look something like this**

```
my $infile=$ARGV[0]; #File to open

open(IN, "<$infile");

while(<IN>)
{
	#do something
}
```

Don't forget that regular expressions can match data by using the following constructs:

If you are matching to the annonymous scalar `$_` you can match with a regular expression bound by `//`

If you want to match something in a scalar variable you you use this instead: 

```
my $var;
if($var=~m/yourtext/);
```

You can also match character classes by bounding the character class with `[ ]`

Thus to search for DNA nucleotides, you can `$var=~m/[ATCGatcg]/`.

Be careful in that this is a "greedy" match, so set up your logic so that it matches fasta ID's first, then sequence data next.

Don't forget that you can match fasta ID lines with `/^>/ of $var=~m/^>/`

**creating @lengths**
Finally, to get the length of a string, you can use the function `length()`;
Note that length will return the length of a sequence INCLUDING THE \n. To remove this and other space characters, process DNA seqeunce data with the regex `s/\s//g`

This will remove all whitespace and prevent length from reporting `\n` as part of the sequence length.

**Sorting @lengths**

Also, to sort an array you can use this code

```
my @x;
@x=sort{$b<=>$a} @x;
```

Note that this is sorting largest to smallest

You can learn more about sort here http://perlmaven.com/sorting-arrays-in-perl


You should get the following number, which is the N50 for Mimulus 2.0
21212587

Bonus points if you can figure out how to print the N90.

Your Perl script should wwork on `/homes/bjsco/Mguttatus_256_v2.0.fa.gz` (this file must be copied to your working directory and gunzipped using `gunzip`) as we have done before.

Your file should work using:

```
./scripts/N50.pl Mguttatus_256_v2.0.fa
```

##Upload your working Perl script to KSOL 

##Problem 2: Parse a GFF3 file

One of the things that Perl programs can do is to parse text files and extract the information that your lab is interested in. This can save you time attempting to manually calculate summaries. Say for example that you have a large tab delimited text file with all annotations of your genome (GFF3 file format). You want to report the average length of features in an article describing your genome. The start and stop coordinates of each individual feature are feilds in the GFF3 format. The "type" of feature is also a feild for each individual feature.

Download the GFF3 formatted file for the [Mimulus genome](https://docs.google.com/file/d/0B_FK3mkgo1L0azFIRGxhVHpnaGs/edit). GFF3 format, or gene feature format, is a way to describe features in a genome. Each line of a GFF3 formatted file contains a single feature found in a locus.

GFF3 format is described here:  http://www.sequenceontology.org/gff3.shtml

A typical entry for a gene will look something like this


```
scaffold_1 phytozome9_0 mRNA 4125905 4131904 . - . ID=PAC:17689354;Name=mgv1a000953m;pacid=17689354;longest=1;Parent=mgv1a000953m.g
scaffold_1 phytozome9_0 CDS 4131641 4131904 . - 0 ID=PAC:17689354.CDS.1;Parent=PAC:17689354;pacid=17689354
scaffold_1 phytozome9_0 CDS 4130504 4131139 . - 0 ID=PAC:17689354.CDS.2;Parent=PAC:17689354;pacid=17689354
scaffold_1 phytozome9_0 CDS 4130164 4130407 . - 0 
```

When processing this type of file you should skip commented lines. In a while loop anything within the following conditional loop will only happen if the line is not a comment:

```
unless (/^#/) 
{
	 do something...
}
```

##Process this data file to print out the following data:

Average gene size

Average Exon (aka CDS) size

Average three_prime_UTR size

Average five_prime_UTR size

**Here is the outline or pseudocode of what your script should do:**

1) Create and edit a script called ~/scripts/average_gff3.pl

2) As you read in lines of the GFF3 file, unless the line is commented, if the type indicates a gene, CDS, three_prime_UTR, or five_prime_UTR then calculate the length of the feature and add its length to an array of lengths for either gene, CDS, three_prime_UTR, or five_prime_UTR features.

3) When this is complete calculate the average length for gene, CDS, three_prime_UTR, or five_prime_UTR features

Your script shoul run with the following command:

./scripts/average_gff3.pl /homes/bioinfo/class/gff3/Mguttatus_140_gene.gff3

##Upload your Perl script to KSOL with the output at the bottom commented out
