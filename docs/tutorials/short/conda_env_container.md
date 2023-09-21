
Let's say you are creating a conda environment named `hisat2`:

```sh
conda create -y -n hisat2-env hisat2=2.2.1
```

!!! note "If you require a single program"
    If you want to use only a single program inside a container, it's best to use the readily available containers at [bioconda.github.io/](https://bioconda.github.io/). E.g. for `hisat2` the singularity command would be:

    ```sh
    singularity pull docker://quay.io/biocontainers/hisat2:2.2.1--h87f3376_4
    ```

!!! note "If using snakemake"
    If you are using conda with snakemake, it's most efficient to convert your environment ymls into a `Dockerfile` with `snakemake --containerize`. For more information, have a look at the [snakemake docs](https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#containerization-of-conda-based-workflows). 

Now you can create an `environment.yml` file that describes the environment and can be used to create the docker container:

```sh
conda activate hisat2-env
conda env export > environment.yml
```

This yaml file will look like this:

```yml
name: hisat2-env
channels:
  - bioconda
  - conda-forge
  - defaults
dependencies:
  - _libgcc_mutex=0.1=conda_forge
  - _openmp_mutex=4.5=2_gnu
  - bzip2=1.0.8=h7f98852_4
  - ca-certificates=2022.12.7=ha878542_0
  - hisat2=2.2.1=h87f3376_4
  - ld_impl_linux-64=2.40=h41732ed_0
  - libffi=3.4.2=h7f98852_5
  - libgcc-ng=12.2.0=h65d4601_19
  - libgomp=12.2.0=h65d4601_19
  - libnsl=2.0.0=h7f98852_0
  - libsqlite=3.40.0=h753d276_0
  - libstdcxx-ng=12.2.0=h46fd767_19
  - libuuid=2.32.1=h7f98852_1000
  - libzlib=1.2.13=h166bdaf_4
  - ncurses=6.3=h27087fc_1
  - openssl=3.0.8=h0b41bf4_0
  - perl=5.32.1=2_h7f98852_perl5
  - pip=23.0=pyhd8ed1ab_0
  - python=3.11.0=he550d4f_1_cpython
  - readline=8.1.2=h0f457ee_0
  - setuptools=67.1.0=pyhd8ed1ab_0
  - tk=8.6.12=h27826a3_0
  - tzdata=2022g=h191b570_0
  - wheel=0.38.4=pyhd8ed1ab_0
  - xz=5.2.6=h166bdaf_0
prefix: /home/gvangeest/miniconda3/envs/hisat2
```

Now, we can prepare the `Dockerfile`:

```dockerfile
FROM continuumio/miniconda3

COPY environment.yml /opt 

RUN conda env create -f /opt/environment.yml

ENTRYPOINT ["conda", "run", "-n", "hisat2-env"]
```

!!! note 
    The name of the environment at `ENTRYPOINT` (`hisat2-env`) should be the same as specified in the `environment.yml` after `name:`. 



!!! note "Building images without entrypoint"
    If you do not want to set an entrypoint (e.g. to use `singularity exec`), you can't use an `environment.yml`, as this will always create a separate environment that is not activated at container start up. An alternative can be to not use `environment.yml`, but add the `conda install` command directly to the `Dockerfile`, and install it in base environment:

    ```dockerfile
    FROM continuumio/miniconda3

    RUN conda install -y -c bioconda hisat2=2.2.1
    ```


In order to create the container image, both the `Dockerfile` and `environment.yml` need to be in the same directory. After that, cd into that directory and run:

```sh
docker build -t [docker hub namespace]/conda-hisat2 .
```

!!! note
    Replace `[docker hub namespace]` with you username on docker hub. E.g. for me this would be:

    ```sh
    docker build -t geertvangeest/conda-hisat2 .
    ```

    If you don't have an account yet on dockerhub, and you want to use dockerhub as a container image repository, make one first. 

Now we can check whether the container does what it should:

```sh
docker run --rm  conda-hisat2 hisat2 --help
```

Which should return the `hisat2` helper. 

In order to share the container, upload it to a container repository (e.g. docker hub):

```sh
docker push [docker hub namespace]/conda-hisat2
```

To use it with singularity you can run:

```sh
singularity pull docker://[docker hub namespace]/conda-hisat2
```

And check wheter it does what is should with:

```sh
singularity run conda-hisat2_latest.sif hisat2 --help
```

!!! warning
    This doens't work with `singularity exec`, as this command replaces the the entrypoint. If you want/need to run `singularity exec`, use:

    ```sh
    singularity exec conda-hisat2_latest.sif conda run -n hisat2-env hisat2 --help
    ```

!!! note "Use gitlab CI!"
    Of course, you can also set up automatic builds with gitlab, which enables you to let gitlab re-create your container at every push/release to your repo. More information at [CI & automatic image builds](../../../tutorials/gitlab/gitlab_ci)
