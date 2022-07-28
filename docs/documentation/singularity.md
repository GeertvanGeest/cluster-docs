Singularity is container software that is particularly useful for compute clusters. In order to get introduced to containers and singularity have a look at this [SIB course](https://sib-swiss.github.io/containers-introduction-training/2022.4/). 

!!! note "Take-away messages"
    - Singularity is only available on compute nodes (not on `binfservms01`)
    - By default only `~` is mounted. Use `--bind` to bind other directories (like `/data`)

On the IBU cluster, `singularity` is only installed on the compute nodes (not on the head node). In order to test whether singularity works for you, start an interactive session with SLURM:

```sh
srun --cpus-per-task=2 --mem=8000 --time=01:00:00 --pty bash 
```

With this interactive session, you will have shell available on a compute node, so we can use `singularity`:

```sh
singularity --help
```

In order to pull an image from dockerhub you can run the following:

```sh
singularity pull docker://ubuntu:latest
```

This will result in a local image called `ubuntu_latest.sif`. 

By default, only your home directory will be mounted to a container:

```sh
singularity exec ubuntu_latest.sif ls ~
```

The above return all files and directories in your home directory. 

However:

```sh
singularity exec ubuntu_latest.sif ls /data
```

Will return:

```
/usr/bin/ls: cannot access '/data': No such file or directory
```

Luckily, you can mount `/data` (or any other directory) with the option `--bind`:

```sh
singularity exec --bind "/data" ubuntu_latest.sif ls /data
```

This will return all files and directories in `/data`. 

!!! note "When using snakemake"
    If you are using singularity containers with the pipeline software `snakemake` you will have to specify this extra argument as well. Do this with the option `--singularity-args`, e.g.:

    ```sh
    snakemake \
    --use-singularity \
    --singularity-args "--bind /data" \
    --cores 2  
    ```