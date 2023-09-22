Apptainer is container software that can be used to run computations inside a container. In order to get introduced to containers and apptainer have a look at this [container tutorial](../tutorials/containers/introduction_containers.md). 

## For the impatient

- Apptainer is only available on compute nodes (not on `binfservms01`)
- By default only `~` is mounted. Use `--bind` to bind other directories (like `/data`)
- Set the variable `APPTAINER_TMPDIR` to a directory with enough disk, e.g. `export APPTAINER_TMPDIR="$SCRATCH"`

## Running apptainer on the IBU cluster

On the IBU cluster, `apptainer` is only installed on the compute nodes (not on the head node). In order to test whether apptainer works for you, start an interactive session with SLURM:

```sh
srun --cpus-per-task=2 --mem=8000 --time=01:00:00 --pty bash 
```

With this interactive session, you will have shell available on a compute node, so we can use `apptainer`:

```sh
apptainer --help
```

## Pulling images

For pulling (large) images from e.g. dockerhub it is important to set the variable `$APPTAINER_TMPDIR`. Otherwise it will write large files to `/tmp`, and this disk will quickly fill up. Your container build will run faster if it is set to the scratch directory associated with your (interactive) job:

```sh
export APPTAINER_TMPDIR="$SCRATCH"
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
apptainer pull docker://ubuntu:latest
```

This will result in a local image called `ubuntu_latest.sif`. 

!!! tip "Add `$APPTAINER_TMPDIR` to your `.bashrc`"
    Every time you run an interactive job, you will have to set `$APPTAINER_TMPDIR` to `$SCRATCH`. However, you might forget that at some point. Therefore, it's best to take the precaution to set the variable to a writable directory on `/data` in your `.bashrc`. In this way, you will never write to `/tmp` by accident: 

    ```sh title="~/.bashrc"
    # apptainer variables
    export APPTAINER_TMPDIR=/data/users/yourusername/
    ```

    It will **always** make sense to run `export APPTAINER_TMPDIR="$SCRATCH"` in an interactive job, as the container building will likely be faster.  

## Mounting directories

By default, only your home directory will be mounted to a container. Luckily, you can mount other directories (e.g. `/data`) with the option `--bind`:

```sh
apptainer exec --bind "/data" ubuntu_latest.sif ls /data
```

If you find yourself often writing `--bind /data` while running apptainer containers, you can use the variable `APPTAINER_BIND`: 

```
export APPTAINER_BIND=/data
```

!!! tip "Add the `$APPTAINER_BIND` to your `.bashrc`"
    For example:

    ```sh title="~/.bashrc"
    # apptainer variables
    export APPTAINER_TMPDIR=/data/users/yourusername/
    export APPTAINER_BIND=/data
    ```

!!! note "When using snakemake"
    If you are using apptainer containers with the pipeline software `snakemake` you will have to specify this extra argument as well. Do this with the option `--apptainer-args`, e.g.:

    ```sh
    snakemake \
    --use-apptainer \
    --apptainer-args "--bind /data" \
    --cores 2  
    ```