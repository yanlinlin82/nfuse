# Introduction #

If you plan to run nFuse directly from a pair of RNA-seq fastq files and a pair of WGSS fastq files, the following is not relevant to you.  If you wish to run nFuse on multiple lanes of data in heterogeneous formats stored in the same directory for each case, the following functionality may be of use.

# Input data formats #

nFuse is designed to be run on source data in any format. Given a correct configuration, nFuse can retrieve multiple lanes of data from a specified directory, convert it from standard formats such as solexa export, qseq, or fastq, to the specific fastq format required by nFuse, and use that data for alignment and prediction. The input data can also be a mix of different formats.

When running nFuse, two command line parameters specify the directories containing input RNA-Seq and genome sequencing data respectively. nFuse searches for filenames matching specific patterns as configured in `config.txt`. Matching filenames are then converted to standard fastq using a conversion tool as configured in `config.txt`.  Tools for conversion from export and qseq formats are provided in the scripts directory.

The following examples from the default `config.txt` should illustrate how to configure nFuse to handle your input data. Some knowledge of perl regular expressions is required.

## Example 1 - Export format ##

<pre>
data_lane_regex_1 = ^(.+)_[12]_export.txt.*$<br>
data_end_regex_1 = ^.+_([12])_export.txt.*$<br>
data_compress_regex_1 = ^.+_[12]_export.txt(.*)$<br>
data_converter_1 = $(scripts_directory)/fq_all2std.pl export2std</pre>

The `data_lane_regex_1` entry tells nFuse to match any filename `*_1_export.txt.*` or `*_2_export.txt.*`, and take the first part of the filename as the lane identifier. For instance `XYZ_1_export.txt.*` will be identified as a file from lane XYZ.

The `data_end_regex_1` entry tells nFuse where in the filename to find the read end. For instance `XYZ_1_export.txt.*` and `XYZ_2_export.txt.*` will contain end 1 and 2 reads from lane XYZ respectively.

The `data_compress_regex_1` entry tells nFuse that the suffix contains the compression type. Currently .bz2 and .gz files as identified by this regular expression will be automatically decompressed on the fly by nFuse. Note that the original files will not be modified.

The `data_converter_1` entry tells nFuse how to convert the uncompressed data to fastq. For export format, we use a modified version of fq\_all2std.pl obtained from the maq software package and provided with the nFuse code. The command specified as the ''converter'' must take the specified data format, in this case export, at the stdin and output fastq format reads to stdout.

## Example 2 - QSEQ format ##

<pre>
data_lane_regex_2 = ^(.+)_[12]_concat_qseq.txt.*$<br>
data_end_regex_2 = ^.+_([12])_concat_qseq.txt.*$<br>
data_compress_regex_2 = ^.+_[12]_concat_qseq.txt(.*)$<br>
data_converter_2 = $(scripts_directory)/qseq2fastq.pl</pre>

The above example for qseq format reads is identical to the export format example, except the `qseq2fastq.pl` script is used to convert to fastq.

## Example 3 - Fastq format ##

<pre>
data_lane_regex_3 = ^(.+).[12].fastq.*$<br>
data_end_regex_3 = ^.+.([12]).fastq.*$<br>
data_compress_regex_3 = ^.+.[12].fastq(.*)$<br>
data_converter_3 = cat</pre>

The above example shows how to use reads already in fastq format with nFuse. A ''converter'' in this case is not necessary, and instead the linux command `cat` is used as a placeholder, since it will take the uncompressed fastq format reads on stdin and pass them, unmodified, to stdout.