**This repository contains instructions on how to run various programs and other useful code.**

### Hardware information for linux
`lscpu` - Reports information about cpu and processing units\
`lshw` - Reports information about different hardware units\
`df -h` - Reports disk information

### General use code
`rename "s/.oldExtension/.newExtension/" *.files_you_want_to_edit` - Mass renaming\
`find $(pwd) -type f > file_list.txt` - Writing file paths to file for a directory\
`sed -i '/^>/s/$/new_text/' file` - Add text after fasta greater than symbol\
`sed -i '/^>/s/ /_/g' file` - Replace all spaces with underscores in a fasta header line\
`rm -v !("filename")` - Remove all files except 'filename'\
`tar -cvzf archive_name.tar.gz /folder/location` - Create a compressed archive\
`tar -xvf arhive.tar.gz` - Decompress archive\
`tar -tvf archive.ar.gz` - View contents of compressed archive\
`tar -zxvf compressed_archive.tar.gz file_to_extract` - Extract specific file from archive\
`tar -zxvf compressed_archive.tar.gz -wildcards '*.wildcard'` - Extract specific file using a wildcard\
`tar -zxvf compressed_archive.tar.gz "file_to_extract" "file_to_extract"` - Extract specific files from archive\
`tar -czf - archive.tar.gz | wc -c` - Check file size of archive\
`gzip -v filename` - Compress a file\
`gzip -d filename.gz` - Decompress file\
`export PATH=$PATH:/path/to/file` - Add this to the .bashrc file to run programs from anywhere\
`awk -F'[field_separator]' '{print $number_of_field}'` - Pipe text into this to set the field separator (can have multiple separators) to pull out a part of the text

### [CheckM](https://github.com/Ecogenomics/CheckM)
```
checkm lineage_wf --reduced_tree -t thread_no -x fasta /path/to/genomes/folder /path/to/output/folder > /path/to/output/folder/checkm_output.txt
```
This produces the standard output for CheckM. You need to run the below command for a more detailed output.
```
checkm qa /path/to/lineage.ms /path/to/output/folder -o 2 -t thread_no --tab_table > /path/to/output/folder/checkm_output.txt
```
#### R plots for CheckM ouput
You can create a variety of plots from the CheckM output using R.
```
library(ggpubr)
library(tidyverse)
```
Density plot of genome size:
```
genome_size_plot = ggplot(data = checkm_data, mapping = aes(x = genome_size)) + geom_density(color = "black", size = 1, fill = "#bf1744") + xlim(2.1, 3.2) + labs(x = "Genome size (Mb)", y = "Density") + theme_classic() + theme(axis.title.y = element_blank())
```
Using `ggpubr` you can aggregate multiple plots:
```
density_plots = ggarrange(genome_size_plot, contig_plot, N50_plot, strain_heterogeneity_plot, labels = c("A", "B", "C", "D"), ncol = 2, nrow = 2, align = "hv")
```
You can then annotate the plots by running the following code:
```
annotated_figures = annotate_figure(density_plots, left = text_grob("Density", rot = 90))
```
### [Prokka](https://github.com/tseemann/prokka)
```
prokka -outdir output_folder -prefix output_file_prefix -locustag prefix_for_genes genome.fasta
```
### [Conda](https://docs.conda.io/projects/conda/en/latest/commands.html)
Working with programs:

`conda config --show channels` - See what channels are in use/
`conda config --add channels channel_name` - Add a channel/
`python --version` - Check python version/
`conda search software_name` - Search for software/
`conda install software_name=version` - Install a program (with specified version)

Working with environments:

`conda create -n environment_name python=python.version` - Create environment (with specified python version)/
`conda activate environment_name` - Activate a conda environment/
`conda deactivate` - Deactivate an environment/
`conda list` - Check the list of software installed in the current environment/
`conda info --envs` - See all environments/
`conda remove -n environment_name --all` - Completely remove environment

### [FastANI](https://github.com/ParBLiSS/FastANI)
Running FastANI comparing one genome to another genome:
```
fastANI -q query.fasta -r reference.fasta -o output_filename.txt -t thread_no
```
Running FastANI comparing one query to multiple references:
```
fastANI -q query.fasta --rl reference_list.txt -o output_filename.txt -t thread_no
```
...where `reference_list.txt` is a text file containing the file paths to all files wanted as references (see 'General use code' section on how to create this file).

Running FastANI comparing multiple queries to multiple references (all vs all):
```
fastANI --ql query_list.txt --rl reference_list.txt -o output_filename.txt -t thread_no
```
...where `query_list.txt` and `reference_list.txt` are text files containing the file paths to the genomes.

### [Tmux](https://github.com/tmux/tmux)

`tmux new -s session_name` - Create a new session
`tmux attach -t session_name` - Attach session
`tmux rename-session -t 0 session_name` - Rename session
`Ctrl-b + d` - Detach from session
`Ctrl-b + c` - Create new window
`Ctrl-b + n` - Next window
`Ctrl-b + p` - Previous window

### [N50](https://github.com/quadram-institute-bioscience/seqfu/wiki/n50)
```
n50 *.fa -f custom -t '{path}{tab}N50={N50}{tab}Sum={size}{tab}Contigs={seqs}{new}' > n50_summary.txt
```
### [MLST](https://github.com/tseemann/mlst)
```
mlst --scheme mlst_scheme genomes > path/to/output/file.txt
```
### [BLAST](https://www.ncbi.nlm.nih.gov/books/NBK279690/)
Making a BLAST database:
```
makeblastdb -in fasta_file -dbtype nucl_or_prot -out database_name
```
Running command-line BLAST:
```
blastn -db database_name -query fasta_file -out results_file.txt -outfmt 6_or"6 extra_columns" -max_target_seqs max_no_of_hits
```


