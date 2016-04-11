# Introduction #

The [nfuse paper](http://genome.cshlp.org/content/early/2012/06/28/gr.136572.111.abstract) describes simulated dataset containing synthetic CGRs.  We provide the code to generate this simulation in a small package.  Download the latest version of the simulation package `cgrsim-0.1.tar.gz` from the Downloads tab.

# Setup #

Download and extract the simulation package.  You will need to edit the configuration file `simconf.txt`.  Set `dataset_directory` to the directory in which you created the reference dataset for nFuse (simply copy the config variable of the same name from your nFuse config).  Additionally set `simulation_dir` to the location in which you would like to store the simulation files, including simulated fastq sequences.

The simulation uses the `maq simulate` command.  Download `maq` and ensure it is in your path.

Next run the following command:

`./create_simulation.pl -c simconf.txt`

After completion, the `simulation_dir` directory should contain `sequencing/dna` and `sequencing/rna` subdirectories that can be used with the `--dnadir` and `--rnadir` command line options of nFuse.

# Evaluation #

After running nFuse on the simulated dataset, you may use the `compute_results.pl` script to analyze the performance of nFuse in identifying the synthetic CGRs.  Run the following comand:

`./compute_results.pl -c simconf.txt -o nfuse/output/dir`

Where `nfuse/output/dir` is the output directory for nFuse, the same `-o` option used for the nFuse command line.

The output of `compute_results.pl` will be a a list of the synthetic CGRs successfully identified by nFuse.  The first field is either `ccbr` or `path` depending on the type of CGR.  The second field is the ccbr id or path id.  The third field is the id of the CGR.  Information about each CGR is contained in text and fasta files in the `simulation_dir` directory.