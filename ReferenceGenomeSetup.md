

# Introduction #

nFuse requires a specific set of files including a genome fasta, gene models GTF and others. The following steps detail how to obtain the required files from ucsc and ensembl. Instructions for hg18 and hg19 are provided.

# hg19 #

## Download the chromosomes from ensembl ##

> `wget ftp://ftp.ensembl.org/pub/release-62/fasta/homo_sapiens/dna/Homo_sapiens.GRCh37.62.dna.chromosome.*.fa.gz`

Delete the ''extra'' chromosomes, such as `Homo_sapiens.GRCh37.62.dna.chromosome.HG1000_2_PATCH.fa.gz`. Do not delete the mitochondrial chromosome `Homo_sapiens.GRCh37.62.dna.chromosome.MT.fa.gz`. Gunzip the remaining fasta files and cat them to a single file. You will also need to remove the description field from each fasta entry. This can be done with the `remove_fasta_description.pl` script in the `scripts` directory. For example:

> `cat Homo_sapiens.GRCh37.62.dna.chromosome.*.fa | remove_fasta_description.pl > Homo_sapiens.GRCh37.62.dna.chromosome.fa`

Set the `genome_fasta` entry in `config.txt` to the name of the fasta file just created.

## Download the gene models from ensembl ##

> `wget ftp://ftp.ensembl.org/pub/release-62/gtf/homo_sapiens/Homo_sapiens.GRCh37.62.gtf.gz`

Gunzip the file and set the `gene_models` entry in `config.txt` to the name of the file just downloaded.

## Download the repeats table from ucsc ##

Navigate to the following URL [http://genome.ucsc.edu/cgi-bin/hgTables?command=start](http://genome.ucsc.edu/cgi-bin/hgTables?command=start). First click at the bottom of the page where it says **To reset all user cart settings (including custom tracks), click here**. Then set `assembly` to **Feb. 2009 (GRCh37/hg19)**, set `group` to **Variation and Repeats**, and set `track` to **RepeatMasker**. Select **gzip compressed** and enter an output file at the bottom of the form. Click `get output`. When the file has finished downloading, gunzip it and set the `repeats_filename` entry in `config.txt` to the name of the file.

## Download the EST fasta from ucsc ##

> `wget ftp://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/est.fa.gz`

Gunzip the file and set the `est_fasta` entry in `config.txt` to the name of the file.

## Download the EST spliced alignments from ucsc ##

> `wget ftp://hgdownload.cse.ucsc.edu/goldenPath/hg19/database/intronEst.txt.gz`

Gunzip the file and set the `est_alignments` entry in `config.txt` to the name of the file.

## Download the Unigene clusters fasta from ncbi ##

> `wget ftp://ftp.ncbi.nih.gov/repository/UniGene/Homo_sapiens/Hs.seq.uniq.gz`

Gunzip the file and set the `unigene_fasta` entry in `config.txt` to the name of the newly created file.

# hg18 #

## Download the chromosomes from ensembl ##

> `wget ftp://ftp.ensembl.org/pub/release-54/fasta/homo_sapiens/dna/Homo_sapiens.NCBI36.54.dna.chromosome.*.fa.gz`

Delete the ''extra'' chromosomes, such as `Homo_sapiens.NCBI36.54.dna.chromosome.c22_H2.fa.gz`. Do not delete the mitochondrial chromosome `Homo_sapiens.NCBI36.54.dna.chromosome.MT.fa.gz`. Gunzip the remaining fasta files and cat them to a single file. You will also need to remove the description field from each fasta entry. This can be done with the `remove_fasta_description.pl` script in the `scripts` directory. For example:

> `cat Homo_sapiens.NCBI36.54.dna.chromosome.*.fa | remove_fasta_description.pl > Homo_sapiens.NCBI36.54.dna.chromosome.fa`

Set the `genome_fasta` entry in `config.txt` to the name of the fasta file just created.

## Download the gene models from ensembl ##

> `wget ftp://ftp.ensembl.org/pub/release-54/gtf/homo_sapiens/Homo_sapiens.NCBI36.54.gtf.gz`

Gunzip the file and set the `gene_models` entry in `config.txt` to the name of the file just downloaded.

## Download the repeats table from ucsc ##

Navigate to the following URL [http://genome.ucsc.edu/cgi-bin/hgTables?command=start](http://genome.ucsc.edu/cgi-bin/hgTables?command=start). First click at the bottom of the page where it says **To reset all user cart settings (including custom tracks), click here**. Then set `assembly` to **Mar. 2006 (NCBI36/hg18)**, set `group` to **Variation and Repeats**, and set `track` to **RepeatMasker**. Select **gzip compressed** and enter an output file at the bottom of the form. Click `get output`. When the file has finished downloading, gunzip it and set the `repeats_filename` entry in `config.txt` to the name of the file.

## Download the EST fasta from ucsc ##

> `wget ftp://hgdownload.cse.ucsc.edu/goldenPath/hg18/bigZips/est.fa.gz`

Gunzip the file and set the `est_fasta` entry in `config.txt` to the name of the file.

## Download the EST spliced alignments from ucsc ##

> `wget ftp://hgdownload.cse.ucsc.edu/goldenPath/hg18/database/*_intronEst.txt.gz`

Gunzip the files and concatenate them into one single file. Set the `est_alignments` entry in `config.txt` to the name of the newly created file.

## Download the Unigene clusters fasta from ncbi ##

> `wget ftp://ftp.ncbi.nih.gov/repository/UniGene/Homo_sapiens/Hs.seq.uniq.gz`

Gunzip the file and set the `unigene_fasta` entry in `config.txt` to the name of the newly created file.

# Creating indices #

Once the required files and tools have been downloaded, the `create_reference_dataset.pl` script will build any derivative files including bowtie indices and 2bit files. Run the following command. Expect this step to take at least 12 hours.

> `create_reference_dataset.pl -c config.txt`