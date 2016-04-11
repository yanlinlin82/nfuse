

# Setup #

A smal amount of initial setup is required to run the nFuse pipeline.  First download the nFuse code and build the toolset.  You will need to make changes to a configuration file to tell nFuse where to put genome indices and where executables reside on your system.  You will then need to run `create_reference_dataset.pl` to create the required indices.

## nFuse code ##

Download and untar/gunzip the nFuse code package to a directory.

### Prerequisites ###

To build the nFuse toolset you must have the <strong>boost c++ development libraries</strong> installed.  If they are not installed on your system you can download them from the [boost website](http://www.boost.org/).  A full install of boost is not required.  The easiest thing to do is to download the latest boost source tar.gz, extract it, then add the extracted path to the CPLUS\_INCLUDE\_PATH environment variable (on bash, `export CPLUS_INCLUDE_PATH=/boost/directory/:$CPLUS_INCLUDE_PATH`)

### Building the toolset ###

In the tools subdirectory of the nFuse package, type `make`.

## External Tools ##

nFuse relies on other publically available tools as part of its pipeline. These tools are not included with the nFuse download. Obtain these tools as detailed below.  Paths to the binaries should be added to  the `config.txt` file in the scripts directory of the nFuse package.

### python2.7 ###

[Python version 2.7](http://www.python.org/download/releases/2.7/) is required, install and edit the python config entry.

### bowtie2 ###

Download and install [bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) and edit the bowtie2-align and bowtie2-build config entries. Must be version
`2.0.0-beta7` or greater.

### blat and faToTwoBit ###

The latest blat tool suite can be downloaded from the ucsc website: [http://hgdownload.cse.ucsc.edu/admin/exe/](http://hgdownload.cse.ucsc.edu/admin/exe/). Download `blat` and `faToTwoBit` and set the `blat_bin` and `fatotwobit_bin` entries in `config.txt` to the fully qualified paths of the `blat` and `faToTwoBit` binaries.

### R ###

The latest version of R can be downloaded from the R project website: [http://www.r-project.org/](http://www.r-project.org/).  Install R and then locate the R and Rscript executables, and set the `r_bin` and `rscript_bin` entries in `config.txt` to the path of those executables.

Install the kernlab and ada packages.  Run R, then at the prompt type `install.packages("kernlab")` and `install.packages("ada")`

### GMAP ###

The latest version of gmap can be downloaded here [http://research-pub.gene.com/gmap/](http://research-pub.gene.com/gmap/).  Build with a default configuration.  Do not worry about the `--with-gmapdb` build flag, nFuse will request a specific directory for the database anyway.

## Reference Dataset ##

Create a directory for storing the reference genome and genome indices.

In the `config.txt` file in the scripts directory, edit the `code_directory` entry to the location of the untarred nFuse code, and edit the `dataset_directory` to the directory you created for storing the reference genome and genome indices.

nFuse requires a specific set of files including a genome fasta, gene models GTF and others. The source files are downloaded automatically.  If automatic downloading fails or you would like to use a different genome, refer to [detailed setup](ReferenceGenomeSetup.md).

### hg19 ###

Set the following configuration variables:

<pre>
ensembl_version                             = 67<br>
ensembl_genome_version                      = GRCh37<br>
ucsc_genome_version                         = hg19<br>
</pre>

### hg18 ###

Set the following configuration variables:

<pre>
ensembl_version                             = 54<br>
ensembl_genome_version                      = NCBI36<br>
ucsc_genome_version                         = hg18<br>
</pre>

### Building the reference dataset ###

Once the required files and tools have been downloaded, the `create_reference_dataset.pl` script will build all required genome indices. Run the following command. Expect this step to take at least 12 hours.

> `create_reference_dataset.pl -c config.txt`

# How to run #

Running the nFuse pipeline is simple as running a single script. To run the nFuse pipeline, run `nfuse.pl` from the scripts directory with the appropriate command line parameters. The parameters are listed below:

<pre>
-h, --help      Displays this information<br>
-c, --config    Configuration Filename<br>
--dnadir        DNA source directory (mutually exclusive of dnafq1, dnafq2)<br>
--dnafq1        DNA fastq end 1 (mutually exclusive of dnadir)<br>
--dnafq2        DNA fastq end 2 (mutually exclusive of dnadir)<br>
--rnadir        RNA source directory (mutually exclusive of rnafq1, rnafq2)<br>
--rnafq1        RNA fastq end 1 (mutually exclusive of rnadir)<br>
--rnafq2        RNA fastq end 2 (mutually exclusive of rnadir)<br>
-o, --output    Output Directory<br>
-n, --name      Library Name<br>
-l, --local     Job Local Directory (default: Output Directory)<br>
-s, --submit    Submitter Type (default: direct)<br>
-p, --parallel  Maximum Number of Parallel Jobs (default: 1)<br>
</pre>

Given paired RNA-seq fastq files rna.1.fastq and rna.2.fastq, and paired genome sequence fastq files dna.1.fastq and dna.2.fastq, run nFuse using the following command:

> `nfuse.pl -c config.txt --dnafq1 dna.1.fastq --dnafq2 dna.2.fastq --rnafq1 rna.1.fastq --rnafq2 rna.2.fastq -o output_dir`

You can also import data from a directory containing multiple lanes of sequencing data as described [here](ImportSetup.md).

Given a machine with multiple processes, 8 for example, run nFuse as follows:

> `nfuse.pl -c config.txt --dnafq1 dna.1.fastq --dnafq2 dna.2.fastq --rnafq1 rna.1.fastq --rnafq2 rna.2.fastq -o output_dir -p 8`

If you have access to a cluster, you may be able to run nFuse as follows for a sun grid engine (SGE) cluster:

> `nfuse.pl -c config.txt --dnafq1 dna.1.fastq --dnafq2 dna.2.fastq --rnafq1 rna.1.fastq --rnafq2 rna.2.fastq -o output_dir -s sge`

or as follows for a portable batch system (PBS) cluster:

> `nfuse.pl -c config.txt --dnafq1 dna.1.fastq --dnafq2 dna.2.fastq --rnafq1 rna.1.fastq --rnafq2 rna.2.fastq -o output_dir -s pbs`

or as follows for a LSF cluster:

> `nfuse.pl -c config.txt --dnafq1 dna.1.fastq --dnafq2 dna.2.fastq --rnafq1 rna.1.fastq --rnafq2 rna.2.fastq -o output_dir -s lsf`

In many cases it is beneficial to store intermediate results on a local disk rather than a network share. This can be done using the `--local` command line parameters as follows:

> `nfuse.pl -c config.txt --dnafq1 dna.1.fastq --dnafq2 dna.2.fastq --rnafq1 rna.1.fastq --rnafq2 rna.2.fastq -o output_dir -s lsf -l /localdisk`

to specify that intermediate files be stored on at /localdisk.

# Output #

nFuse produces 3 output files: `rna.results.txt`, `dna.cycle.results.tsv`, and `dna.paths.results.tsv` corresponding to fusion transcript predictions, simplex and complex breakpoint predictions, and closed chains of breakage and rejoining predictions respectively.  All 3 files are tab separated flat files.  The following fields are common to all 3 files:

<ul>
<blockquote><li><strong>cluster_id</strong> : unique identifier of the fusion transcript or breakpoint prediction</li>
<li><strong>adjacent</strong> : fusion between adjacent genes</li>
<li><strong>deletion</strong> : fusion produced by a genomic deletion</li>
<li><strong>eversion</strong> : fusion produced by a genomic eversion</li>
<li><strong>gene1</strong> : ensembl id of gene 1</li>
<li><strong>gene2</strong> : ensembl id of gene 2</li>
<li><strong>gene_name1</strong> : name of gene 1</li>
<li><strong>gene_name2</strong> : name of gene 2</li>
<li><strong>gene_chromosome1</strong> : chromosome of gene 1</li>
<li><strong>gene_chromosome2</strong> : chromosome of gene 2</li>
<li><strong>gene_strand1</strong> : strand of gene 1</li>
<li><strong>gene_strand2</strong> : strand of gene 2</li>
<li><strong>gene_start1</strong> : start of gene 1</li>
<li><strong>gene_start2</strong> : start of gene 2</li>
<li><strong>gene_end1</strong> : end position for gene 1</li>
<li><strong>gene_end2</strong> : end position for gene 2</li>
<li><strong>gene_location1</strong> : location of breakpoint in gene 1</li>
<li><strong>gene_location2</strong> : location of breakpoint in gene 2</li>
<li><strong>genome_breakseqs_percident</strong> : maximum percent identity of fusion sequence alignments to genome</li>
<li><strong>genomic_break_pos1</strong> : genomic position in gene 1 of fusion splice / breakpoint</li>
<li><strong>genomic_break_pos2</strong> : genomic position in gene 2 of fusion splice / breakpoint</li>
<li><strong>genomic_strand1</strong> : genomic strand in gene 1 of fusion splice / breakpoint, retained sequence upstream on this strand, breakpoint is downstream</li>
<li><strong>genomic_strand2</strong> : genomic strand in gene 2 of fusion splice / breakpoint, retained sequence upstream on this strand, breakpoint is downstream</li>
<li><strong>interchromosomal</strong> : fusion produced by an interchromosomal translocation</li>
<li><strong>inversion</strong> : fusion produced by genomic inversion</li>
<li><strong>library_name</strong> : library name given on the command line of nFuse</li>
<li><strong>min_map_count</strong> : minimum of the number of genomic mappings for each spanning read</li>
<li><strong>max_map_count</strong> : maximum of the number of genomic mappings for each spanning read</li>
<li><strong>mean_map_count</strong> : average of the number of genomic mappings for each spanning read</li>
<li><strong>num_multi_map</strong> : number of spanning reads that map to more than one genomic location</li>
<li><strong>repeat_list1</strong> : list of types of repeats overlapped by spanning reads in gene 1</li>
<li><strong>repeat_list2</strong> : list of types of repeats overlapped by spanning reads in gene 2</li>
<li><strong>repeat_proportion1</strong> : proportion of the spanning reads in gene 1 that span a repeat region</li>
<li><strong>repeat_proportion2</strong> : proportion of the spanning reads in gene 2 that span a repeat region</li>
<li><strong>repeat_proportion_max</strong> : max of repeat_proportion1 and repeat_proportion2</li>
<li><strong>sequence</strong> : predicted breakpoint or fusion sequence</li>
<li><strong>span_count</strong> : number of spanning reads supporting the fusion</li>
<li><strong>span_coverage1</strong> : coverage of spanning reads aligned to gene 1 as a proportion of expected coverage</li>
<li><strong>span_coverage2</strong> : coverage of spanning reads aligned to gene 2 as a proportion of expected coverage</li>
<li><strong>span_coverage_min</strong> : minimum of span_coverage1 and span_coverage2</li>
<li><strong>span_coverage_max</strong> : maximum of span_coverage1 and span_coverage2</li>
</ul></blockquote>

## Fusion Transcript Predictions ##

The fusion transcript predictions from nFuse will be stored in the tab separated `rna.results.txt` file in the output directory.  Each row corresponds to a unique fusion transcript predictions.  Fusion transcript predicted to be genomic in origin will provide the id of an associated simple or complex breakpoint (CCBR) prediction.  In addition to common fields, the following fields are present:

<ul>
<blockquote><li><strong>altsplice</strong> : fusion likely the product of alternative splicing between adjacent genes</li>
<li><strong>break_adj_entropy1</strong> : di-nucleotide entropy of the 40 nucleotides adjacent to the fusion splice in gene 1</li>
<li><strong>break_adj_entropy2</strong> : di-nucleotide entropy of the 40 nucleotides adjacent to the fusion splice in gene 2</li>
<li><strong>break_adj_entropy_min</strong> : minimum of break_adj_entropy1 and break_adj_entropy2</li>
<li><strong>breakpoint_homology</strong> : number of nucleotides at the fusion splice that align equally well to gene 1 or gene 2</li>
<li><strong>cdna_breakseqs_percident</strong> : maximum percent identity of fusion sequence alignments to cdna</li>
<li><strong>defuse_probability</strong> : probability of a fusion transcript existing as calculated using deFuse</li>
<li><strong>est_breakseqs_percident</strong> : maximum percent identity of fusion sequence alignments to est</li>
<li><strong>estisland_breakseqs_percident</strong> : maximum percent identity of fusion sequence alignments to est islands</li>
<li><strong>exonboundaries</strong> : fusion splice at exon boundaries</li>
<li><strong>expression1</strong> : expression of gene 1 as number of concordant pairs aligned to exons</li>
<li><strong>expression2</strong> : expression of gene 2 as number of concordant pairs aligned to exons</li>
<li><strong>gene_align_strand1</strong> : alignment strand for spanning read alignments to gene 1</li>
<li><strong>gene_align_strand2</strong> : alignment strand for spanning read alignments to gene 2</li>
<li><strong>interrupted_index1</strong> : ratio of coverage before and after the fusion splice / breakpoint in gene 1</li>
<li><strong>interrupted_index2</strong> : ratio of coverage before and after the fusion splice / breakpoint in gene 2</li>
<li><strong>num_splice_variants</strong> : number of potential splice variants for this gene pair</li>
<li><strong>orf</strong> : fusion combines genes in a way that preserves a reading frame</li>
<li><strong>readthrough</strong> : fusion involving adjacent potentially resulting from co-transcription rather than genome rearrangement</li>
<li><strong>splice_score</strong> : number of nucleotides similar to GTAG at fusion splice</li>
<li><strong>splicing_index1</strong> : number of concordant pairs in gene 1 spanning the fusion splice / breakpoint, divided by number of spanning reads supporting the fusion with gene 2</li>
<li><strong>splicing_index2</strong> : number of concordant pairs in gene 2 spanning the fusion splice / breakpoint, divided by number of spanning reads supporting the fusion with gene 1</li>
<li><strong>splitreads_count</strong> : number of split reads supporting the prediction</li>
<li><strong>splitreads_min_pvalue</strong> : p-value, lower values are evidence the prediction is a false positive</li>
<li><strong>splitreads_pos_pvalue</strong> : p-value, lower values are evidence the prediction is a false positive</li>
<li><strong>splitreads_span_pvalue</strong> : p-value, lower values are evidence the prediction is a false positive</li>
</ul></blockquote>

## Simple and Complex Breakpoint Predictions ##

The simple and complex breakpoint predictions from nFuse will be stored in the tab separated `dna.paths.results.tsv` file in the output directory.  Each row corresponds to a breakpoint prediction (rows may be duplicated if involved in multiple complex breakpoints).  Each complex breakpoint is represented by multiple rows with the same `path_id`.  In addition to common fields, the following fields are present:

<ul>
<blockquote><li><strong>path_id</strong> : unique identifier of the complex breakpoint prediction</li>
<li><strong>path_score</strong> : score for the path taking into account lengths of genomic shards and probability of breakpoints (lowest for a given set of splice variants) </li>
<li><strong>path_length</strong> : total length of genomic shards and intronic regions (lowest for a given set of splice variants) </li>
</ul></blockquote>

## Closed Chains of Breakage and Rejoining Predictions ##

The CCBR predictions from nFuse will be stored in the tab separated `dna.cycles.results.tsv` file in the output directory.  Each row corresponds to a breakpoint prediction (rows may be duplicated if involved in multiple CCBRs).  Each CCBR is represented by multiple rows with the same `ccbr_id`.  In addition to common fields, the following fields are present:

<ul>
<blockquote><li><strong>cycle_id</strong> : unique identifier of the CCBR</li>
<li><strong>cycle_score</strong> : score for the CCBR taking into account distances between breakpoints and breakpoint probabilities</li>
<li><strong>cycle_length</strong> : total distance between breakpoints</li>
</ul>