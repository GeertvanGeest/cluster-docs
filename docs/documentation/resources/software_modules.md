

## Vital-IT modules

    http://www.vital-it.ch/services/software

## Vital-IT singularity containers

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

## Locally installed software

`/mnt/apps/`

## Spack

[https://projects.bioinformatics.unibe.ch/projects/ibu-best-practices/wiki/spack](https://projects.bioinformatics.unibe.ch/projects/ibu-best-practices/wiki/spack)

## IGV

  /mnt/apps/centos7/IGV\_2.4.10. See [IGVOnCluster.pdf](https://projects.bioinformatics.unibe.ch/attachments/18/IGVOnCluster.pdf) below for instructions on how to use IGV on the cluster (from Windows).
