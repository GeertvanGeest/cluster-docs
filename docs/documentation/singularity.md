Singularity is container software that is particularly useful for compute clusters. In order to get introduced to containers and singularity have a look at this [SIB course](https://sib-swiss.github.io/containers-introduction-training/2022.4/). 

## TL;DR

- Singularity is only available on compute nodes (not on `binfservms01`)
- By default only `~` is mounted. Use `--bind` to bind other directories (like `/data`)
- Set the variable `SINGULARITY_TMPDIR` to a directory with enough disk, e.g. `export SINGULARITY_TMPDIR=/data/users/yourusername/`

## Running singularity on the IBU cluster

On the IBU cluster, `singularity` is only installed on the compute nodes (not on the head node). In order to test whether singularity works for you, start an interactive session with SLURM:

```sh
srun --cpus-per-task=2 --mem=8000 --time=01:00:00 --pty bash 
```

With this interactive session, you will have shell available on a compute node, so we can use `singularity`:

```sh
singularity --help
```

## Pulling images

For pulling (large) images from e.g. dockerhub it is important to set the variable `$SINGULARITY_TMPDIR`. Otherwise it will write large files to `/tmp`, and this disk will quickly fill up. Set it to e.g. your personal directory in `/data/users/`:

```sh
export SINGULARITY_TMPDIR=/data/users/yourusername/
```

In order to pull an image from dockerhub you can run the following:

```sh
singularity pull docker://ubuntu:latest
```

This will result in a local image called `ubuntu_latest.sif`. 

## Mounting directories

By default, only your home directory will be mounted to a container. Luckily, you can mount other direcotries (e.g. `/data`) with the option `--bind`:

```sh
singularity exec --bind "/data" ubuntu_latest.sif ls /data
```

If you find yourself often writing `--bind /data` while running singularity containers, you can use the variable `SINGULARITY_BIND`: 

```
export SINGULARITY_BIND=/data
```

!!! tip "Add the variables to your `.bashrc`"
    For example:

    ```sh title="~/.bashrc"
    # singularity variables
    export SINGULARITY_TMPDIR=/data/users/yourusername/
    export SINGULARITY_BIND=/data
    ```

!!! note "When using snakemake"
    If you are using singularity containers with the pipeline software `snakemake` you will have to specify this extra argument as well. Do this with the option `--singularity-args`, e.g.:

    ```sh
    snakemake \
    --use-singularity \
    --singularity-args "--bind /data" \
    --cores 2  
    ```