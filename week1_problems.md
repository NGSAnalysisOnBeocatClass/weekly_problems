##Unix shell basics

The goal of this week's problem is to use what you've learned about understanding usage statements, reading manuals/help menus, identifying NGS file extensions and writting commands to run programs that may be new to you. Make use of the help menus, the Github FAQ pages and the class google+ group while you work. 

You will download a bacterial genome as well as single-end illumina reads from RNA. Read quality will be checked. Reads will be aligned to the genome. Then summary statistics describing the alignment will be generated.

#####RECORD ALL COMMANDS THAT YOU RUN IN A TEXT FILE USING TEXTWRANGLER OR NOTEPAD++ AS THE EDITOR. UPLOAD THIS TO KSOL WHEN YOU HAVE COMPLETED THE ASSIGNMENT. YOU WILL BE GRADED ON THIS PLAIN TEXT FILE. YOU WILL KNOW WHEN YOU HAVE A COMMAND TO WRITE AND RUN WHEN YOU SEE AN INSTRUCTION IN RED TEXT LIKE THE ONE BELOW:

```javascript
"Run your own command to ..."
```

### Step 1: Download your data

NCBI hosts a FTP that you can use to download genomes, transcriptomes, blast databases or even the raw reads that were used in papers. Visit the FTP at ftp://ftp.ncbi.nlm.nih.gov. Read about how to use `wget` to download directly from an FTP at https://github.com/i5K-KINBRE-script-share/FAQ/blob/master/GetRawData.md or by using the `man` program.

```javascript
"Run your own command to create a directory named `hw1` in your home directory."
```

Using your knowledge of file extensions and the `wget` command download a copy of the _E. coli_ genome for strain K-12 substrain MG1655 to your `~/hw1` directory. The genome fasta file is one of the files found at  ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/Escherichia_coli_K_12_substr__MG1655_uid57779/. You can view all of the files in this FTP directory by pasting ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/Escherichia_coli_K_12_substr__MG1655_uid57779/ into a web browser or clicking on the hyperlink.

**Hint: The `Escherichia_coli_K_12_substr__MG1655_uid57779` directory has no `README` file to tell you which files are which. This is poor form but you will see this kind of thing often. You have to use your knowledge of NGS data file extensions to find the genome fasta file. If you right-click (control-click on Macs) on the fasta file you can copy the full URL from a web browser.**

```javascript
"Run your own command using wget to download the genome fasta file to your ~/hw1 directory"
```

Next you will download illumina RNA reads from this URL ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR136/SRR1363869/SRR1363869.sra. 


```javascript
"Run your own command using wget to download the .sra file to your ~/hw1 directory"
```

Read about how to decompress short read archieve (`.sra`) files at the bottom of https://github.com/i5K-KINBRE-script-share/FAQ/blob/master/GetRawData.md.

```javascript
"Run your own command using fastq-dump to download the .sra file to your ~/hw1 directory"
```

### Step 2 : Quality check your data

Fastq files contain phred scale (-10 log<sub>10</sub>) base quality scores that are estimates of base quality. Before using fastq reads you should evaluate read quality (base quality, number of reads, check for tags, etc). Prinseq is a progam written in Perl to check read quality or to clean reads to remove low quality bases. Read more about checking read quality and Prinseq at http://prinseq.sourceforge.net/manual.html.

If you would like to generate a help menu for either prinseq-lite.pl or prinseq-graph.pl type:

```
perl /homes/bioinfo/bioinfo_software/prinseq-lite.pl -help

perl /homes/bioinfo/bioinfo_software/prinseq-graphs.pl -help
```

Below is the typical usage statement to quality check single-end reads. You will notice that commands in the prinseq manual and these usage statments start with `perl`. One way to run programs is to specify the interpreter (in this case `perl`) and then call the program (in this case `/homes/bioinfo/bioinfo_software/prinseq-lite.pl` and `/homes/bioinfo/bioinfo_software/prinseq-graphs.pl`).

```
USAGE: perl /homes/bioinfo/bioinfo_software/prinseq-lite.pl -verbose -fastq [fastq file] -graph_data -out_good null -out_bad null
```

```javascript
"Run your own command using prinseq-lite.pl to quality check your reads"
```

Below is the typical usage statement to create `.html` graphs of your read quality on Beocat. The -i flag stands for input. Your last command created a file with the same fullpath as your fastq file with the extension `.gd`. List your `~/hw1` directory to see this file. This is the input file you should specify for the `-i` flag.

```
USAGE: perl /homes/bioinfo/bioinfo_software/prinseq-graphs.pl -verbose -i [fastq_file.gd] -html_all
```

```javascript
"Run your own commands to generate read quality graphs in your ~/hw1 directory"
```

The final graphs of read quality should be at `~/hw1/` and have the file extension `.html`. You can download this file from Beocat and veiw it in a web browser. 

Use the prinseq manual to evaluate your results http://prinseq.sourceforge.net/manual.html.

###Step 3: Index your genome

`bowtie2-build` is a program to index a fasta file so that reads can be aligned to it quickly. You can read a detailed list of parameter options for bowtie2-build by typing `/homes/bioinfo/bioinfo_software/bowtie2-2.1.0/bowtie2-build -h`.

Below is the Bowtie2 usage statement with definitions from the help menu:

```
Usage: bowtie2-build [options]* <reference_in> <bt2_index_base>
    reference_in            comma-separated list of files with ref sequences
    bt2_index_base          write .bt2 data to files with this dir/basename
```

Below is a typical usage statement for building an index file of your reference fasta and then aligning single-end reads with bowtie2-build on Beocat:

```
USAGE: /homes/bioinfo/bioinfo_software/bowtie2-2.1.0/bowtie2-build [genome fasta file] [index files' basename with no file extension]
```

```javascript
"Run your own command to generate an index of your fasta genome in your ~/hw1 directory"
```

###Step 4: Map your reads to the indexed genome

`bowtie2` is a program to map or align reads to a reference. `bowtie2` outputs `.sam` format alignment files. You can read a detailed list of parameter options for Bowtie2 by typing `/homes/bioinfo/bioinfo_software/bowtie2-2.1.0/bowtie2 -h` or by visiting their manual http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml.

Below is the Bowtie2 usage statement with definitions from the help menu:

```
Usage:
  bowtie2 [options]* -x <bt2-idx> {-1 <m1> -2 <m2> | -U <r>} [-S <sam>]

  <bt2-idx>  Index filename prefix (minus trailing .X.bt2).
             NOTE: Bowtie 1 and Bowtie 2 indexes are not compatible.
  <m1>       Files with #1 mates, paired with files in <m2>.
             Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).
  <m2>       Files with #2 mates, paired with files in <m1>.
             Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).
  <r>        Files with unpaired reads.
             Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).
  <sam>      File for SAM output (default: stdout)

  <m1>, <m2>, <r> can be comma-separated lists (no whitespace) and can be
  specified many times.  E.g. '-U file1.fq,file2.fq -U file3.fq'.
  
```

Below is a typical usage statement for aligning single-end reads with Bowtie2 to your indexed genome on Beocat:

```
/homes/bioinfo/bioinfo_software/bowtie2-2.1.0/bowtie2  -q -x [index files basename with no file extension] -U [fastq file] -S [output sam file ending in .sam]
```

```javascript
"Run your own command to generate a SAM file of the reads aligned to your indexed genome in your ~/hw1 directory"
```

###Step 5: Pipes and filters with samtools 

During class we learned about SAM sequence alignment format files and BAM the binary version of the same thing. Samtools is a popular bioinformatics package with many commands that manipulate SAM and BAM files.

Many samtools commands are tailored to work with SAM/BAM format files but they work like Unix commands (e.g. they can be joined using `|` to avoid printing uneeded intermediate files) and to speed up small jobs. **When piping a workflow for samtools commands use `-` in place of input or output files to indicate to read from standard in or to write to standard out.** It is better not to use pipes on extremely large Bam or Sam files. **Text that is bracketed with square brakets ([ ]) or diamond brakets (< >) represents user specified input**.

Here would be typical usage:

       Usage:   samtools <command> [options]
       
Here would be typical piped usage:

       Usage:   samtools <command> [options] [input] - | samtools <command> [options] - - | samtools <command> [options] - [output]

However, pipes depend on the program reading in from standard in and sending output to standard out by default. A few samtools have a different default behavior. Because of this a couple samtools commands should be treated differently.

**samtools view**

`samtools view` outputs to standard out by default so you should exclude the `-` that indicates output file.

       Usage at begining of pipe:   samtools view [options] [input] |
       
       Usage in middle of pipe:   | samtools view [options] - |
       
       
**samtools sort**

Without the -o parameter nothing is output to standard out by `samtools sort`. This means the next step in the piped workflow gets no input. The `-o` parameter for `samtools sort` indicates that samtools should "Output the final alignment to the standard output".

       Usage at begining of pipe:   samtools sort [options] [input] -o - |
       
       Usage in middle of pipe:   | samtools sort [options] -o - - |

Write a command with pipe to compress your sam file to bam, sort it, remove PCR duplicates, and report simple statistics on the de-duplicated bam.

Run samtools without any arguments to see which commands to use:

       /homes/bioinfo/bioinfo_software/samtools/samtools 
       
Run samtools with a command but no arguments to see a detailed help menu for individual commands or read a detailed manual at http://samtools.sourceforge.net/samtools.shtml#3. 

Below I have added the `S` option (aka parameter) to the `samtools view` command. The `S` parameter indicates "input is in SAM." I have also added the `u` option to the `samtools view` command which specifies "output in the uncompressed BAM format". I would have used the `b` for "output in BAM format in place of `u` if I was not piping my workflow. Using pipes and `u` speeds up the workflow below.

```
/homes/bioinfo/bioinfo_software/samtools/samtools view -Sb [test.sam] -o [test.bam] 

/homes/bioinfo/bioinfo_software/samtools/samtools sort [test.bam] [test_sort]

/homes/bioinfo/bioinfo_software/samtools/samtools rmdup [test_sort.bam] [test_rmdup.bam]

/homes/bioinfo/bioinfo_software/samtools/samtools flagstat [test_rmdup.bam]
```

```javascript
"Run your own piped command that completes all four steps above to generate a flagstat summary of your alignments"
```

