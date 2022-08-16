Singularity is container software that is particularly useful for compute clusters. In order to get introduced to containers and singularity have a look at this [SIB course](https://sib-swiss.github.io/containers-introduction-training/2022.4/). 

## For the impatient

- Singularity is only available on compute nodes (not on `binfservms01`)
- By default only `~` is mounted. Use `--bind` to bind other directories (like `/data`)
- Set the variable `SINGULARITY_TMPDIR` to a directory with enough disk, e.g. `export SINGULARITY_TMPDIR="$SCRATCH"`

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

For pulling (large) images from e.g. dockerhub it is important to set the variable `$SINGULARITY_TMPDIR`. Otherwise it will write large files to `/tmp`, and this disk will quickly fill up. Your container build will run faster if it is set to the scratch directory associated with your (interactive) job:

```sh
export SINGULARITY_TMPDIR="$SCRATCH"
```

!!! info "Do this from within a job"
    The variable `$SCRATCH` only exists inside an (interactive) job. Check it with:

    ```sh
    echo "$SCRATCH"
    ```

    This should return something like:

    ```
    /scratch/8946748
    ```

    If it doesn't return anything, you're not in an interactive job. 

In order to pull an image from dockerhub you can run the following:

```sh
singularity pull docker://ubuntu:latest
```

This will result in a local image called `ubuntu_latest.sif`. 

!!! tip "Add `$SINGULARITY_TMPDIR` to your `.bashrc`"
    Every time you run an interactive job, you will have to set `$SINGULARITY_TMPDIR` to `$SCRATCH`. However, you might forget that at some point. Therefore, it's best to take the precaution to set the variable to a writable directory on `/data` in your `.bashrc`. In this way, you will never write to `/tmp` by accident: 

    ```sh title="~/.bashrc"
    # singularity variables
    export SINGULARITY_TMPDIR=/data/users/yourusername/
    ```

    It will **always** make sense to run `export SINGULARITY_TMPDIR="$SCRATCH"` in an interactive job, as the container building will likely be faster.  

## Mounting directories

By default, only your home directory will be mounted to a container. Luckily, you can mount other directories (e.g. `/data`) with the option `--bind`:

```sh
singularity exec --bind "/data" ubuntu_latest.sif ls /data
```

If you find yourself often writing `--bind /data` while running singularity containers, you can use the variable `SINGULARITY_BIND`: 

```
export SINGULARITY_BIND=/data
```

!!! tip "Add the `$SINGULARITY_BIND` to your `.bashrc`"
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