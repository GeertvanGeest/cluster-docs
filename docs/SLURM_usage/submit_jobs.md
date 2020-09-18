# SLURM job submission

## Introduction

A cluster has many resources and many users. Often the demand for resources is higher than supply. A job scheduler can schedule the jobs submitted by the different users, based on priority, required resources and time. The IBU cluster uses [SLURM](https://slurm.schedmd.com) as job scheduler. After submitting a job to the cluster, SLURM will try to fulfill the job's resource request by allocating resources to the job.

## Resource Allocation

Every job submission starts with a resources allocation (nodes, cores, memory). Allocations are created by most users with the `sbatch` and `srun` commands. Most users will use `sbatch` with or without `srun`.

### sbatch
The `sbatch` command is used to submit a job script for later execution. SLURM options can be part of the submission command, but they are usually in the job script and prefixed by `#SBATCH`.

#### Basic usage
A job submission script start with the options for SLURM prefixed with `#SBATCH`. These are used to manage the resources (e.g. memory) and configure the job environment. All the options for job allocation can be found at [Allocation options](allocation_options.md). The SLURM options are followed by the actual job script.

Here's an example, `first_job.sh`:

```Bash
#!/bin/bash
#SBATCH --job-name="first_job"
#SBATCH --time=00:10:00
#SBATCH --mem-per-cpu=4G

./calc_mat.sh
```

Submit the job script:

```bash
[username@binfservms01 ~]$ sbatch first_job.sh
Submitted batch job 34534
```

Jobs get a job ID that is printed to stdout after job submission. In this case it is 34534. We can use this number to track back our job, e.g. for [status monitoring](SLURM_usage/status_monitoring.md).

#### Parallelization with arrays
In bioinformatics, you will often need to run a similar command with a range of parameters, e.g. running an alignment of multiple files with reads. SLURM makes this easy with the `--array` option. The array option generates a [special variable for SLURM](SLURM_usage/slurm_variables.md), namely `$SLURM_ARRAY_TASK_ID`. This variable is replaced for each number given to the array.

Basic usage:

```bash
#!/bin/bash
#SBATCH --job-name=test_emb_arr
#SBATCH --output=res_emb_arr.txt
#SBATCH --ntasks=1
#SBATCH --time=10:00
#SBATCH --mem-per-cpu=100
#SBATCH --array=1-8

my_program $SLURM_ARRAY_TASK_ID
```

This will do a parameter sweep for my_program with the parameters 1 till 8. It generates separate jobs and allocations for each item in the array. Each job will inherit the options given to `sbatch` by the `#SBATCH` prefix.

This concept can become very useful in the combination with UNIX arrays (yes, same name, different thing..). Let's say you have a directory with input files (e.g. sequence reads), and you want run a program on them (e.g. quality control). By generating a UNIX array of the file names and index them based on `$SLURM_ARRAY_TASK_ID`, you can run it in parallel with only a few lines of code:

```bash
#!/bin/bash
#
#SBATCH --job-name=test_emb_arr
#SBATCH --output=res_emb_arr.txt
#SBATCH --ntasks=1
#SBATCH --time=10:00
#SBATCH --mem-per-cpu=100
#SBATCH --array=0-7

## make an array of all files a directory
FILES=(/path/to/input_data/*)

## run my_program on the ith item in the UNIX array
my_program ${FILES[$SLURM_ARRAY_TASK_ID]}
```

!!! note "UNIX uses zero indexing"
    Like python and many other languages UNIX uses zero-based indexing, meaning the first item in a list is indicated by a 0, the second by 1 etc. So, in the example above the SLURM array should range from 0-7 if there are 8 files in your input directory.

### srun
The command `srun` submits a job for execution in real time, and takes most of the same options as `sbatch` described at [Allocation options](allocation_options.md). You need it if you want to make use of job steps or parallelization with a Message Passing Interface (MPI). Because it runs in real-time, after calling `srun` the terminal will be blocked untill the job is finished. Usually `srun` is used within a script submitted with `sbatch` to generate job steps and/or parallelization. In the case `srun` is used within a `sbatch` script, the `sbatch` options are inherited by `srun`. A detailed discussion  on the difference between `sbatch` and `srun` can be found at [stackoverflow](https://stackoverflow.com/questions/43767866/slurm-srun-vs-sbatch-and-their-parameters).

#### Generating job steps
Within a script submitted with `sbatch`, `srun` creates job steps.

The script below, `jobsteps.sh`, generates two job steps with `srun`:

```bash
#!/bin/bash

#SBATCH --time=00:10:00
#SBATCH --partition=pall
#SBATCH --job-name=test_slurm

srun hostname
srun sleep 60
```
With default values of `sacct` (more on this at [status monitoring](SLURM_usage/status_monitoring.md)) you can get basic information on job steps. Here you can see which job steps have run, together with their allocated CPUs, status and exit code:

```Bash
[username@binfservms01 ~]$ sbatch jobsteps.sh
Submitted batch job 6200897
[username@binfservms01 ~]$ sacct -j 6200897
       JobID    JobName  Partition    Account  AllocCPUS      State ExitCode
------------ ---------- ---------- ---------- ---------- ---------- --------
6200897      test_slurm       pall  username          1  COMPLETED      0:0
6200897.bat+      batch             username          1  COMPLETED      0:0
6200897.0      hostname             username          1  COMPLETED      0:0
6200897.1         sleep             username          1  COMPLETED      0:0
```

#### Parallelization of job steps
As you remember, `sbatch` options are inherited by `srun`. Therefore without extra options to `srun` overriding it, a job step has access to every CPU allocated to the job created by `sbatch`. How multiple job steps divide the allocated resources can be arranged with the option `--ntasks` and `--exclusive`. Using `--ntasks` for `sbatch` defines how many tasks can be allocated at the same time for that job, for `srun` this means the number of tasks per job step. With the option `--exclusive` to `srun` you make sure each task can make full use of the allocated CPU and not share it with other job steps.

!!! danger "Don't use `--exclusive` in combination with `sbatch`"
    Using `--exclusive` with `sbatch` leads to exclusive usage of an entire node. This is almost never necessary, and leads to very high usage of the available resources.

In the below example we generate a job with 4 tasks. However, the code generates 10 job steps with the for-loop. By defining the options `--ntasks=1` and `--exclusive` we make sure only four job-steps can run at the same time with a full CPU each. The remaining jobs will wait untill the other jobs are finised.

```bash
#!/bin/bash

#SBATCH --time=00:10:00
#SBATCH --ntasks=4
#SBATCH --mem-per-cpu=4G
#SBATCH --cpus-per-task=1
#SBATCH --partition=pall
#SBATCH --job-name=test_slurm

for i in {1..10}
do
  srun --ntasks=1 --exclusive touch $i.tst &
done
wait
```

#### Interactive jobs

Performing heavy calculations on the home node is not a good practice. However, for debugging, or small/simple steps, you might want to perform relatively heavy jobs interactively. With `srun` you can allocate a job and use bash to interactively call commands within the allocation.

With the script below we allocate resources with 4 CPUs, 4G memory per CPU restricted to a time of 4 hours. With the option `--pty` we say that we want to perform the task in 'pseudo-terminal mode', and the terminal of our choice is bash.

```bash
[username@binfservms01 ~]$ srun --cpus-per-task=4 --mem-per-cpu=4000 --time=04:00:00 --pty bash
srun: job 6200830 queued and waiting for resources
srun: job 6200830 has been allocated resources
[username@binfservas32 ~]$
```

!!! note "Different node"
    Note that the new shell is not at the home node `binfservms01` but at a compute node assigned by SLURM: `binfservas32`
