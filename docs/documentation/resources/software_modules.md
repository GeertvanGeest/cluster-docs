

## Vital-IT

!!! bug "This probably needs to be moved or redirection"
    Whole part on this at the HPC tutorial

All modules can be found here:

[http://www.vital-it.ch/services/software](http://www.vital-it.ch/services/software)

### Vital-IT singularity containers

singularity is installed only on nodes, but not on ms01. Containers can be run via bash-scripts submitted to slurm or in an interactive slurm session.  
start the slurm session:

```sh
srun --pty --mem=20G /bin/bash
```

display help:

```sh
singularity run /software/singularity/containers/qiime2-2018.8-1.debian9.simg
```

run the tool _import_ from QIIME:

```sh
singularity exec /software/singularity/containers/qiime2-2018.8-1.debian9.simg qiime tools import --param1 someParam --param2 someParam
```

### Locally installed software

!!! bug "This needs to be elaborated"

`/mnt/apps/`

### Spack

!!! bug "This probably needs elaboration"

[https://projects.bioinformatics.unibe.ch/projects/ibu-best-practices/wiki/spack](https://projects.bioinformatics.unibe.ch/projects/ibu-best-practices/wiki/spack)

### IGV

!!! bug "This part contains missing links"

!!! bug "This probably needs elaboration"

!!! bug "This probably needs to be moved to a different location"

/mnt/apps/centos7/IGV\_2.4.10. See [IGVOnCluster.pdf](https://projects.bioinformatics.unibe.ch/attachments/18/IGVOnCluster.pdf).
