

## Vital-IT

A lot of software is pre-installed as modules on the server. In the [SLURM tutorial](../../../tutorials/SLURM_tutorial/#4-modules) it is explained how to use these modules.

All modules can be found here:
[http://www.vital-it.ch/services/software](http://www.vital-it.ch/services/software)

### Vital-IT singularity containers

Singularity is installed on all nodes, but not on ms01. Containers can be run via bash-scripts submitted to slurm or in an interactive slurm session.

In order to test or debug a singularity container, start an interactive slurm session:

```sh
srun --pty --mem=20G /bin/bash
```

Usually, you can run the software after the singularity executable like you're used to via the command line. E.g. running the tool _import_ from QIIME would look like this:

```sh
singularity exec /software/singularity/containers/qiime2-2018.8-1.debian9.simg qiime tools import --param1 someParam --param2 someParam
```

You can find SIB course material on how to use docker and singularity [here](https://sib-swiss.github.io/containers-introduction-training/).

### Locally installed software

!!! bug "This needs to be elaborated"

`/mnt/apps/`

### Spack

The following has to be run before using spack on the IBU cluster:

`source /mnt/cluster/software/bin/ibuspack.sh`

Easiest is to put this line in your bashrc file.

To see all spack modules currently installed on our cluster

```text
module avail
```

To see all tools that are available as spack modules and could be installed:

`spack list`
