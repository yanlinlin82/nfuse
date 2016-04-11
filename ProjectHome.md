# Introduction #
Complex Genomic Rearrangements (CGRs) are an important aspect of the genomic landscape of many tumours.  CGRs often disrupt genes and fuse other genes, in some cases promoting tumour development.

nFuse predicts fusion transcripts and associated CGRs from matched RNA-seq and Whole Genome Shotgun Sequencing (WGSS).  See the [getting started](GettingStarted.md) guide for instructions on how to setup and run nFuse.

# Publication #

[nFuse: Discovery of complex genomic rearrangements in cancer using high-throughput sequencing.](http://genome.cshlp.org/content/22/11/2250)
McPherson AW, Wu C, Wyatt A, Shah SP, Collins C, Sahinalp SC.
Genome Res. 2012 Jun 28.

# News #

## Don't Use Ensembl 67 - 01/19/2013 ##
  * If you study prostate cancer, Ensembl 67 (and possibly other versions) have an erroneous gene encompassing TMPRSS2 and ERG, and you will miss the TMPRSS2-ERG fusion
  * Ensembl 70 does not have the same issue

## Version 0.2.0 Available - 12/07/2012 ##
  * Now using bowtie and SSE2 accelerated realignment for discordant alignments
  * Improved calculation of posterior probability of discordant alignments
  * Added non-chromosomal genomic sequences to reference
  * Added quality trimming step
  * Assigning reads to breakpoints in setcover to reduce false positives from fragmentation of larger clusters
  * Optimizations for clustering step

## Simulation Available - 10/08/2012 ##
  * Download version 0.1 from Downloads tab
  * [documentation available here](CGRSimulation.md)

## Version 0.1.4 Available - 09/28/2012 ##
  * Fixes undefined $genome\_cdna\_fasta
  * Speedups in alignment probability, clustering and connectivity statistics
  * More filtering options
  * Better documentation in config.txt

## Version 0.1.3 Available - 03/08/2012 ##
  * Fixes crash in `import_fastq.pl`

## Version 0.1.2 Available - 17/07/2012 ##
  * Partial alignment for RNA-seq
  * `discord_read_trim` replaced with `min_partial_align_size`
  * fixed get\_reads.pl
  * no longer calculating `calculating splicing_index`, `interrupted_index`, `expression` (if you previously found these useful, email me and I may try to re-implement them efficiently)

## Move from sourceforge - 17/07/2012 ##
We have moved over from our sourceforge site.  Current version is 0.1.1 and can be downloaded from the Downloads tab.

# Acknowledgements #

|![http://nfuse.googlecode.com/files/sfu_logo.png](http://nfuse.googlecode.com/files/sfu_logo.png) | ![http://nfuse.googlecode.com/files/VPC_Logo.png](http://nfuse.googlecode.com/files/VPC_Logo.png) | ![http://nfuse.googlecode.com/files/bccancer_logo.png](http://nfuse.googlecode.com/files/bccancer_logo.png) |
|:-------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------|