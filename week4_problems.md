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

####Subroutine 1) Get conserved portion of header

-- Write a subroutine to substitute a variable `$header_line` with everything in the header but `/1` or `/2` and the newline. Re-read Chapter 9 "Processing Text with Regular Expressions" in the Perl text if you need a refresher on substitutions or regex. You can also use http://regexpal.com/. 

-- Return the new `$header_line`.

####Subroutine 2) Conatenate three lines of a file (don't remove newlines)

-- Make a subroutine to create a new variable `$next_lines`. Append the next three lines in a file to `$next_lines`. 

-- To do this you will be using the diamond operator in a new way. We have seen `$variable=<STDIN>` and `while (<$fh>)`. Take a look at `/homes/sheltonj/solutions/diamond/diamond.pl`.

-- Read the code in the program and run it with:

```
  /homes/sheltonj/solutions/diamond/diamond.pl /homes/sheltonj/solutions/diamond/test.txt
```
-- Use what you learn to write subroutine 2.

####Input/Output Files)

-- Open a forward read file and a reverse read file from the first and second argument passed to your script as read only `<`.

-- Create output filenames with that begin with the directory `output/` and has `_good` added to the basename using `fileparse` from the same [perl module](https://github.com/NGSAnalysisOnBeocatClass/homework/blob/master/homework_6.md#step-3-create-your-output-file) as you used in the last homework.

-- Create an output filename that begins with the directory `output/` and includes the basename of one input file, the number (`1` or `2`) of the other and `_singletons`.

####Make a hash of forward reads

While there are lines in the forward read file:

-- run subroutine 1.

-- Make a key in the hash for the the return value.

-- Make the value be the text that was removed from the header. 

-- Run subroutine 2 and append the output of this to the end of value.

####Make a hash of reverse reads

While there are lines in the reverse read file:

-- do the same things for them as you did with forward reads.

-- if a key exists in both hashes print both keys and values to thier respective forward or reverse output read files and `delete` the keys and values from the hashes

-- if not print the reverse key and value to the singleton output read file and `delete` the key and value from the reverse read hash

####Find the forward file's singletons

For any remaining key in the forward hash:

-- print the forward key and value to the singleton output read file

####Your job should run with the command:

```
broken_pair.pl ~/class/fastq/broken_pair/broken_R1.fastq ~/class/fastq/broken_pair/broken_R2.fastq
```

