# IBU cluster documentation

## Introduction
The IBU cluster the main computer of cluster of the [Interfaculty Bioinformatics Unit](https://www.bioinformatics.unibe.ch/) at the [University of Bern](https://www.unibe.ch/). This documentation is largely based on the documentation of the [Ubelix cluster](https://ubelix.unibe.ch/), and documentation of the [CÉCI cluster](http://www.ceci-hpc.be/)

## Working on the IBU cluster & new users
We assume all users have a good knowledge of UNIX command line, basic knowledge on HPC computing and job submission using [SLURM](https://slurm.schedmd.com/documentation.html). If you are doubting whether you possess the right skills follow the [UNIX e-learning module at SIB](https://edu.sib.swiss/pluginfile.php/2878/mod_resource/content/4/couselab-html/content.html), and one of our courses on HPC computing.

## System infrastructure
The system consists in total of 1888 cores spread over 34 compute nodes. These nodes are divided into three different partitions.

| PARTITION | NODES |
|-----------|-------|
| pall      | 30    |
| pshort    | 2     |
| phighmem  | 1     |
