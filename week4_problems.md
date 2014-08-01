## Fix broken pairs

A single entry of a fastq file should look like this:

```
@SRR014849.259 EIXKN4201AG62Q/2
TTGCAGGGTTAAGAGCGTTA
+
B;:==D@.B;B;=6==<B;=
```

Fastq format is described well here: http://en.wikipedia.org/wiki/FASTQ_format

- The first line with `@` is the sequence ID and a sequence identifier. 

- The second line is the read itself, for this example, they are very short reads

- The third line starts generally with a `+`. Otherwise it is a second header.

- The 4th line is the quality string. While beyond the scope of this exercise, the base call qualities are encoded in in ASCII characters. This line should always look like gibberish.

If your reads are paired the header line should end in `/1` or `/2`. If it does not then you should use the `convert_headers.sh` script we created in [Code challenge 4](https://github.com/NGSAnalysisOnBeocatClass/in_class_problems/blob/master/Lecture3.md#code-challenge-4) to convert post Casava 1.8 Illumina headers to end in `/1` or `/2`.

Many bioinformatics tools use the `/1` or `/2` headers to find a read's pair. Those that do not use "pair order". 

Pair order means that your reads are in two files. One file is forward reads (generally this has a 1 in the filename). The second file is reverse reads (this has a 2 in the filename). The first read in the forward file is the "pair" of the first read in the second file and so on for each read.

It is good practice to check pair order if someone has cleaned your reads with a bioinformatics tool like FastQC that is not "pair aware" or if you suspect that someone else has processed the fastq files but you don't know what they did.

If pair order is violated then you should correct the order and remove "broken pairs". Broken pairs are reads that no longer have a pair in one of the two files. These are typically moved to a file with "singletons" in the name.

##Weekly problem 4 pseudo code:

For the last weekly programming problem we will use a hash, a regex and subroutines to fix fastq files with broken pairs.

Create and edit a file called `broken_pair.pl`.

####Step 1) Get conserved section of header

Write a subroutine to substitute a variable `$header_line` with everything in the header but `/1` or `/2` and the newline. Return the new `$header_line`.

####Step 2) 

Make a subroutine to create a new variable `$next_lines`. For `(1..3)` append the next line in a file to `$next_lines`. 

To do this you will be using the diamond operator in a new way. We have seen `$variable=<STDIN>` and `while <$fh>`


