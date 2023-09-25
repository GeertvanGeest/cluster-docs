
!!! note "Experienced users"
        Are you a more experienced user and looking for some specific SLURM documentation? Have a look at the [SLURM usage page](../documentation/SLURM_usage/submit_jobs.md)

## How we work:

* For each chapter (2 till 5) we'll have a few slides of presentation and exercises. We'll discuss the exercises and continue with the next chapter when most of you have finished the exercises of the current chapter.
* You're doing this to learn, so don't be tempted to look at the answers to early. If the given answer doesn't correspond to your answer, try to understand why.
* If you're stuck, ask your colleagues first. Someone who has just learned, is often better able to explain it than a teacher.

## Material

[:fontawesome-solid-file-pdf: Download the presentation](../assets/pdf/SLURM_and_modules.pdf){: .md-button }

## 1. Setting up VSCode to work on the IBU cluster

To work conveniently on the cluster we will set up VScode. VScode is a code editor that can be used to edit files and run commands locally, but also on a remote server. In this subchapter we will set up VScode to work on the IBU cluster.

!!! note "Required installations"
    For this exercise you will need to have installed [VScode](https://code.visualstudio.com/download). In addition you would need to have followed the instructions to set up remote-ssh:
    
    - [OpenSSH compatible client](https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client). This is usually pre-installed on your OS.
    - The Remote-SSH extension. To install, open VSCode and click on the extensions icon (four squares) on the left side of the window. Search for `Remote-SSH` and click on `Install`.

Open VScode and click on the green or blue button in the bottom left corner. Select `Connect to Host...`, and then on `Configure SSH Host...`. Specify a the location for the config file. Use the same directory as where your keys are stored (so `~/.ssh`). A skeleton config file will be provided. Edit it, so it looks like this:

=== "Windows"
    ```
    Host ibucluster
            User username
            HostName binfservms01.unibe.ch
            IdentityFile ~\.ssh\id_rsa
    ```
=== "MacOS/Linux"
    ```
    Host ibucluster
            User username
            HostName binfservms01.unibe.ch
            IdentityFile ~/.ssh/id_rsa
    ```

Save and close the config file. Now click again the green or blue button in the bottom left corner. Select `Connect to Host...`, and then on `ibucluster`. You will be asked which operating system is used on the remote. Specify 'Linux'. If your private key is password protected, and you don't have the ssh agent running (this will be true for most Linux/Mac OS users), you will be asked for your private key password. 

Now that you have connected to the cluster, you will end up in your home directory on the head node (`/home/yourusername`). However, we want to work in your personal directory on `/data`. This directory is at `/data/users/yourusername`. First, we will create a working directory called `slurm_tutorial` in your personal directory. Write in the terminal:

```sh
mkdir -p /data/users/yourusername/slurm_tutorial
```

After that, open this directory as a working directory in VSCode by clicking the blue button 'Open Folder' or by going to 'File' > 'Open Folder'. Type the path to the directory `slurm_tutorial` in your personal directory (`/data/users/yourusername/slurm_tutorial`). You will now be able to edit files in this directory.

## 2. SLURM introduction

 <iframe width="640" height="360" src="https://tube.switch.ch/embed/bfbb68ea" frameborder="0" allow="fullscreen" allowfullscreen></iframe>


!!! note
    From now on you will work in VSCode. Make sure you have set `/data/users/yourusername/slurm_tutorial` as your working directory. 

**Exercise 2A:** Create a script named `sleep.sh` (File > New Text File). Write a command in it that lets the system sleep for 120 seconds (use the command `sleep`).

??? done "Answer"
    Your script should look like this:
    ```sh title="sleep.sh"
    #!/usr/bin/env bash

    sleep 120
    ```

You could submit this script as a job to the cluster with the following command:

```bash
sbatch \
--cpus-per-task=1 \
--mem-per-cpu=100M \
--time=00:05:00 \
sleep.sh
```

**Exercise 2B:** Rewrite the script `sleep.sh` in a way that you integrate the SLURM options in the script (hint: use `#SBATCH`).

??? done "Answers"
    Your script should look like this:
    ```sh title="sleep.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=100M
    #SBATCH --time=00:05:00

    sleep 120
    ```

**Exercise 2C:** Submit your script with integrated `sbatch` options to the cluster. Check out the status of the job (use `squeue`).

??? done "Answer"
    Sumbit it like this:
    ```sh
    sbatch sleep.sh
    ```

    Check out the status:
    ```
    squeue --job [JOBID]
    ```
    or
    ```
    squeue -A [USERNAME]
    ```

## 3. sbatch options

<iframe width="640" height="360" src="https://tube.switch.ch/embed/e01c25f4" frameborder="0" allow="fullscreen" allowfullscreen></iframe>

### 3.1 Required resources

**Exercise 3.1A:** The command `sleep` requires none or very little computational resources. Modify the options `--time` and `--mem-per-cpu` to minimize the allocation. Submit it to the cluster with `sbatch` to see if it still works.

??? done "Answer"
    Your script could look like this:
    ```sh title="sleep.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=1M
    #SBATCH --time=00:02:10

    sleep 120
    ```

### 3.2 User specific options

**Exercise 3.2A:** Add options to `sleep.sh` that:

* gives it a name
* provide your e-mail address
* e-mails you when it begins and ends

Submit it to the cluster, and answer the following questions based on the information in the e-mails:

* How long was your job queued?
* On which node did it run?
* How long did the job take?

??? done "Answer"
    Your script should look like this:
    ```sh title="sleep.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=1M
    #SBATCH --time=00:02:10
    #SBATCH --job-name=go_to_sleep
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end

    sleep 120
    ```

    * The queued time is in the subject of the first e-mail.
    * The node you can find in the second e-mail at `NodeList`
    * The time the job took is also in the second e-mail at `Elapsed`

### 3.3 Output and error

**Exercise 3.3A:** Create a new script called `output_and_error.sh` that:

* writes a message to stdout (hint: use `echo`).
* generates an error by using `ls` on a file that doesn't exist.

Run it at the login node. Where do stdin and stdout end up?

??? done "Answer"
    Your script should look like this:
    ```sh title="output_and_error.sh"
    #!/usr/bin/env bash

    echo "I am output!"

    ls my_nonexisting_file.txt
    ```

    The output and error end up both at the console.

**Exercise 3.3B:** Add sbatch options to `output_and_error.sh` that we have learned before (job name, email, cpu, memory and time) with sensible parameters. Now also include `--output` and `--error`. Use `%j` to associate the files with the job ID.

Submit it with sbatch. Were your expected files created? Where did stdout and stderr end up?

??? done "Answer"
    Your script should look like this:
    ```sh title="output_and_error.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=100M
    #SBATCH --time=00:01:00
    #SBATCH --job-name=output_and_error
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/data/users/[USER]/slurm_tutorial/output_%j.o
    #SBATCH --error=/data/users/[USER]/slurm_tutorial/error_%j.e

    echo "I am output!"

    ls my_nonexisting_file.txt
    ```

    Stdout should be in `/data/users/[USER]/slurm_tutorial/output_[JOBID].o` and stderr in `/data/users/[USER]/slurm_tutorial/error_[JOBID].e`

## 4. Interactive jobs

 <iframe width="640" height="360" src="https://tube.switch.ch/embed/cc9095ef" frameborder="0" allow="fullscreen" allowfullscreen></iframe>

**Exercise 4A:** Create interactive job with `srun`. Use the options `--cpus-per-task`, `--mem-per-cpu` and `--time`. Allocate  maximum 1 CPU, 500 MB memory for 5 minutes.

* What is your job ID?
* To which node is your job submitted?

??? done "Answer"
    Your command should look like this:

    ```sh
    srun --cpus-per-task=1  --mem-per-cpu=500M --time=00:05:00 --pty bash
    ```

    You can find the job ID and node with `squeue`. However the node you're on also appears in the shell, e.g.:
    ```sh
    [gvangeest@binfservms01 ~]$
    ```

    becomes something different, e.g.:

    ```sh
    [gvangeest@binfservas07 ~]$
    ```

## 5. Modules and containers

 <iframe width="640" height="360" src="https://tube.switch.ch/embed/c64a439a" frameborder="0" allow="fullscreen" allowfullscreen></iframe>

**Exercise 5A:** Load the module for the latest version of `minimap2` (look it up here: [https://www.vital-it.ch/services](https://www.vital-it.ch/services)), and check out the help documentation with `minimap2 --help`.

??? done "Answer"
    The command to load minimap2 is:
    ```
    module add UHTS/Analysis/minimap2/2.17
    ```

**Exercise 5B:** Start an interactive job with `srun`, and allocate little resources. Is minimap2 still available in your interactive job?

??? done "Answer"
    Yes, it should still be available. If a job is submitted, the current environment of the user is used for that job to run in. However, in a script, it is good practice to always include the `module add` commands, to make your script re-usable and sharable.

We'll now be working with some actual biological data. Go to `/data/courses/HPC_tutorial/ecoli/` and see what's in there.

**Exercise 5C:** Submit a job to run `minimap2` to align sequence reads of sample 1 to the reference `ecoli-strK12-MG1655.fasta`. Call the script `run_minimap2.sh`. Use the sbatch options for cpu, memory, time, job name, e-mail, output and error. This job requires:

* 3 CPU
* 200M of memory per cpu
* 1 minute

You can find the commands to run `minimap2` below. Add the sbatch options and `module add` command to this script and submit to  the cluster.

!!! warning "Warning"
    Do **not** run this on the head node!

```sh title="run_minimap2.sh"
REFERENCE_DIR=/data/courses/HPC_tutorial/ecoli/reference
READS_DIR=/data/courses/HPC_tutorial/ecoli/reads
ALIGN_DIR=/data/users/[USER]/slurm_tutorial/ecoli/alignment

mkdir -p $ALIGN_DIR

cd $READS_DIR

minimap2 \
-a \
-x sr \
$REFERENCE_DIR/ecoli-strK12-MG1655.fasta \
ecoli_sample1.fastq \
> $ALIGN_DIR/ecoli_sample1.fastq.sam
```

??? done "Answer"
    Your script should look like this:
    ```sh title="run_minimap2.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=3
    #SBATCH --mem-per-cpu=200M
    #SBATCH --time=00:01:00
    #SBATCH --job-name=align_reads
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/data/users/[USER]/slurm_tutorial/output_alignment_%j.o
    #SBATCH --error=/data/users/[USER]/slurm_tutorial/error_alignment_%j.e

    module add UHTS/Analysis/minimap2/2.17

    REFERENCE_DIR=/data/courses/HPC_tutorial/ecoli/reference
    READS_DIR=/data/courses/HPC_tutorial/ecoli/reads
    ALIGN_DIR=/data/users/[USER]/slurm_tutorial/ecoli/alignment

    mkdir -p $ALIGN_DIR

    cd $READS_DIR

    minimap2 \
    -a \
    -x sr \
    $REFERENCE_DIR/ecoli-strK12-MG1655.fasta \
    ecoli_sample1.fastq \
    > $ALIGN_DIR/ecoli_sample1.fastq.sam
    ```

As you have learned in the [apptainer exercises](containers/apptainer.md), you can run applications in a container. We have created a container with `minimap2` in it. You can find it at `/data/courses/HPC_tutorial/containers/minimap2.sif`. We'll use this container to run `minimap2` in the next exercise.

**Exercise 5D:** Do the same job submission as in exercise 4C, but now run the command in a container with apptainer. Make sure to add the option `--bind /data/courses` to the `apptainer exec` command, because by default the container has no access to the `/data/courses` directory.

!!! hint
    A command to run an application in a container looks like this:

    ```sh
    apptainer exec --bind /data/courses /path/to/container.sif command
    ```

??? done "Answer"
    The script looks the same as in exercise 4C, but now with the `apptainer exec` command:
    ```sh title="run_minimap2.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=3
    #SBATCH --mem-per-cpu=200M
    #SBATCH --time=00:01:00
    #SBATCH --job-name=align_reads
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/data/users/[USER]/slurm_tutorial/output_alignment_%j.o
    #SBATCH --error=/data/users/[USER]/slurm_tutorial/error_alignment_%j.e

    module add UHTS/Analysis/minimap2/2.17

    REFERENCE_DIR=/data/courses/HPC_tutorial/ecoli/reference
    READS_DIR=/data/courses/HPC_tutorial/ecoli/reads
    ALIGN_DIR=/data/users/[USER]/slurm_tutorial/ecoli/alignment

    mkdir -p $ALIGN_DIR

    cd $READS_DIR

    apptainer exec \
    --bind /data/courses \
    /data/courses/HPC_tutorial/containers/minimap2.sif \
    minimap2 \
    -a \
    -x sr \
    $REFERENCE_DIR/ecoli-strK12-MG1655.fasta \
    ecoli_sample1.fastq \
    > $ALIGN_DIR/ecoli_sample1.fastq.sam
    ```

## 6 Job arrays

 <iframe width="640" height="360" src="https://tube.switch.ch/embed/5e1e7ca8" frameborder="0" allow="fullscreen" allowfullscreen></iframe>
 
### 6.1 Jobs in parallel

**Exercise 6.1A:** Generate a script `array.sh` with the sbatch options for cpu, memory, time, job name, e-mail, output and error. Now also include the option to initiate an array counting from 10 till 15. In the script, let each element of the array create a file with the `$SLURM_ARRAY_TASK_ID` in it's name in your home directory. In order to check out `squeue`, let the system sleep for 60 seconds after generating the file.

??? hint "Hint"
    Your script (without sbatch options) should look something like this:
    ```sh title="array.sh"
    touch /data/users/[USER]/slurm_tutorial/file_$SLURM_ARRAY_TASK_ID.txt
    sleep 60
    ```

* How many jobs were created?
* How many error and output files were created?

??? done "Answer"
    Your script should like like this:
    ```sh title="array.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=1M
    #SBATCH --time=00:02:00
    #SBATCH --job-name=arrays
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/data/users/[USER]/slurm_tutorial/output_%j.o
    #SBATCH --error=/data/users/[USER]/slurm_tutorial/error_%j.e
    #SBATCH --array=10-15

    touch /data/users/[USER]/slurm_tutorial/file_$SLURM_ARRAY_TASK_ID.txt
    sleep 60
    ```

    * Six jobs were created
    * Six output and six error files are created

### 6.2 Using UNIX arrays

**Exercise 6.2A:** Modify the script in exercise 4C to align the reads of all three samples in separate jobs. Use the sbatch `--array` option in combination with a UNIX array, and submit it to the cluster. Check out the output files.

??? hint "Hint"
    Modify the script in exercise 4C with the following steps:

    * Add the option `--array` to the sbatch options (remember: there are three files and UNIX starts counting at 0)
    * Make the directory with reads the current directory, and generate variable with a UNIX array containing the read files. (Use `()` and `*`, e.g. `FILES=(*.fastq)`)
    * Replace the read file name that is used as input for `minimap2` with something like `${FILES[$SLURM_ARRAY_TASK_ID]}`
    * Add `.sam` to the variable file name to modify the output file name.

??? done "Answer"
    Your script should look like this:
    ```sh title="run_minimap2.sh"
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=3
    #SBATCH --mem-per-cpu=200M
    #SBATCH --time=00:01:00
    #SBATCH --job-name=align_reads
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/data/users/[USER]/slurm_tutorial/output_alignment_%j.o
    #SBATCH --error=/data/users/[USER]/slurm_tutorial/error_alignment_%j.e
    #SBATCH --array=0-2

    module add UHTS/Analysis/minimap2/2.17

    REFERENCE_DIR=/data/courses/HPC_tutorial/ecoli/reference
    READS_DIR=/data/courses/HPC_tutorial/ecoli/reads
    ALIGN_DIR=/data/users/[USER]/slurm_tutorial/ecoli/alignment

    mkdir -p $ALIGN_DIR

    cd $READS_DIR

    FILES=(*.fastq)
    
    apptainer exec \
    --bind /data/courses \
    /data/courses/HPC_tutorial/containers/minimap2.sif \
    minimap2 \
    -a \
    -x sr \
    $REFERENCE_DIR/ecoli-strK12-MG1655.fasta \
    ${FILES[$SLURM_ARRAY_TASK_ID]} \
    > $ALIGN_DIR/${FILES[$SLURM_ARRAY_TASK_ID]}.sam    
    ```
