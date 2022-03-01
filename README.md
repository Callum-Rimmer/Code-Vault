**This repository contains instructions on how to run various programs and other useful code**

### Hardware information for linux
`lscpu` - Reports information about cpu and processing units\
`lshw` - Reports information about different hardware units\
`df -h` - Reports disk information

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
