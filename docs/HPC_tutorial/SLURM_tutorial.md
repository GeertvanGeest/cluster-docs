## Material

[:fontawesome-solid-file-pdf: Download the presentation](../assets/pdf/SLURM_and_modules.pdf){: .md-button }

## SLURM introduction

**Exercise:** Create a script named `sleep.sh` that let's the system sleep for 120 seconds (use the command `sleep`).

??? done "Answer"
    Your script should look like this:
    ```sh
    #!/usr/bin/env bash

    sleep 120
    ```

You could submit this script as a job to the cluster with the following command:

```
sbatch \
--cpus-per-task=1 \
--mem-per-cpu=100M \
--time=00:05:00 \
sleep.sh
```

**Exercise**: Rewrite the script `sleep.sh` in a way that you integrate the SLURM options in the script (hint: use `#SBATCH`).

??? done "Answers"
    Your script should look like this:
    ```sh
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=100M
    #SBATCH --time=00:05:00

    sleep 120
    ```

**Exercise:** Submit your script with integrated `sbatch` options to the cluster. Check out the status of the job (use `squeue`).

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

## sbatch options

### 1. Required resources

**Exercise:** The command `sleep` requires none or very little computational resources. Modify the options `--time` and `--mem-per-cpu` to minimize the allocation. Submit it to the cluster with `sbatch` to see if it still works.

??? done "Answer"
    Your script could look like this:
    ```sh
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=1M
    #SBATCH --time=00:02:10

    sleep 120
    ```

### 2. User specific options

**Exercise:** Add options to `sleep.sh` that:

* gives it a name
* provide your e-mail address
* e-mails you when it begins and ends

Submit it to the cluster, and answer the following questions based on the information in the e-mails:

* How long was your job queued?
* On which node did it run?
* How long did the job take?

??? done "Answer"
    Your script should look like this:
    ```sh
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=1M
    #SBATCH --time=00:02:10
    #SBATCH --job-name=go_to_sleep
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end

    sleep 120
    ```

    * The queued time is in the subject first e-mail.
    * The node you can find in the second e-mail at `NodeList`
    * The time the job took is also in the second e-mail at `Elapsed`

### 3. Output and error

**Exercise:** Create a new script called `output_and_error.sh` that:

* writes a message to stdout (hint: use `echo`).
* generates an error by using `ls` on a file that doesn't exist.

Run it at the login node. Where do stdin and stdout end up?

??? done "Answer"
    Your script should look like this:
    ```sh
    #!/usr/bin/env bash

    echo "I am output!"

    ls my_nonexisting_file.txt
    ```

    The output and error end up both at the console.

**Exercise:** Add sbatch options to `output_and_error.sh` that we have learned before (job name, email, cpu, memory and time) with sensible parameters. Now also include `--output` and `--error`. Use `%j` to associate the files with the job ID.

Submit it with sbatch. Were your expected files created? Where did stdout and stderr end up?

??? done "Answer"
    Your script should look like this:
    ```sh
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=100M
    #SBATCH --time=00:01:00
    #SBATCH --job-name=output_and_error
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/home/user/output_%j.o
    #SBATCH --error=/home/user/error_%j.e

    echo "I am output!"

    ls my_nonexisting_file.txt
    ```

    Stdout should be in `/home/user/output_[JOBID].o` and stderr in `/home/user/error_[JOBID].e`

### 4. Interactive jobs

**Exercise:** Create interactive job with `srun`. Use the options `--cpus-per-task`, `--mem-per-cpu` and `--time`. Allocate  maximum 1 CPU, 500 MB memory for 5 minutes.

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

## Modules

**Exercise:** Load the module for the latest version of `minimap2` (look it up here: [https://www.vital-it.ch/services](https://www.vital-it.ch/services)), and check out the help documentation with `minimap2 --help`.

??? done "Answer"
    The command to load minimap2 is:
    ```
    module add UHTS/Analysis/minimap2/2.17
    ```

**Exercise:** Start an interactive job with `srun`, and allocate little resources. Is minimap2 still available in your interactive job?

??? done "Answer"
    Yes, it should still be available. If a job is submitted, the current environment of the user is used for that job to run in. However, in a script, it is good practice to always include the `module add` commands, to make your script re-usable and sharable.

**Exercise:** Submit a job to run `minimap2` to align sequence reads of sample 1 to the refence `ecoli-strK12-MG1655.fasta`. Use the sbatch options for cpu, memory, time, job name, e-mail, output and error. This job requires:

* 3 CPU
* 200M of memory per cpu
* 1 minute

You can find the commands to run `minimap2` below. Add the sbatch options and `module add` command to this script and submit to  the cluster.

!!! warning "Warning"
    Do **not** run this on the head node!

```sh
REFERENCE_DIR=/home/gvangeest/Courses/HPC_course/ecoli/reference
READS_DIR=/home/gvangeest/Courses/HPC_course/ecoli/reads
ALIGN_DIR=/home/gvangeest/Courses/HPC_course/ecoli/alignment

mkdir $ALIGN_DIR

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
    ```sh
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=3
    #SBATCH --mem-per-cpu=200M
    #SBATCH --time=00:01:00
    #SBATCH --job-name=align_reads
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/home/user/output_alignment_%j.o
    #SBATCH --error=/home/user/error_alignment_%j.e

    module add UHTS/Analysis/minimap2/2.17

    REFERENCE_DIR=/home/gvangeest/Courses/HPC_course/ecoli/reference
    READS_DIR=/home/gvangeest/Courses/HPC_course/ecoli/reads
    ALIGN_DIR=/home/gvangeest/Courses/HPC_course/ecoli/alignment

    mkdir $ALIGN_DIR

    cd $READS_DIR

    minimap2 \
    -a \
    -x sr \
    $REFERENCE_DIR/ecoli-strK12-MG1655.fasta \
    ecoli_sample1.fastq \
    > $ALIGN_DIR/ecoli_sample1.fastq.sam
    ```

## Job arrays

### 1. Jobs in parallel

**Exercise:** Generate a script `array.sh` with the sbatch options for cpu, memory, time, job name, e-mail, output and error. Now also include the option to initiate an array counting from 10 till 15. In the script, let each element of the array create a file with the `$SLURM_ARRAY_TASK_ID` in it's name in your home directory. In order to check out `squeue`, let the system sleep for 60 seconds after generating the file.

??? hint "Hint"
    Your script (without sbatch options) should look something like this:
    ```
    touch /home/user/file_$SLURM_ARRAY_TASK_ID.txt
    sleep 60
    ```

* How many jobs were created?
* How many error and output files were created?

??? done "Answer"
    Your script should like like this:
    ```sh
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=1
    #SBATCH --mem-per-cpu=1M
    #SBATCH --time=00:02:00
    #SBATCH --job-name=arrays
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/home/user/output_%j.o
    #SBATCH --error=/home/user/error_%j.e
    #SBATCH --array=10-15

    touch /home/user/file_$SLURM_ARRAY_TASK_ID.txt
    sleep 60
    ```

    * Six jobs were created
    * Six output and six error files are created

### 2. Using UNIX arrays

**Exercise:** Align the reads of the three samples in separate jobs. Use the sbatch `--array` option in combination with a UNIX array, and submit it to the cluster. Check out the output files.

??? hint "Hint"
    Modify the script in the following steps:

    * Add the option `--array` to the sbatch options (remember: there are three files and UNIX starts counting at 0)
    * Make the directory with reads the current directory, and generate variable with a UNIX array containing the read files. (Use `()` and `*`, e.g. `FILES=(*.fastq)`)
    * Replace the read file name that is used as input for `minimap2` with something like `${FILES[$SLURM_ARRAY_TASK_ID]}`
    * Add `.sam` to the variable file name to modify the output file name.

??? done "Answer"
    Your script should look like this:
    ```sh
    #!/usr/bin/env bash

    #SBATCH --cpus-per-task=3
    #SBATCH --mem-per-cpu=200M
    #SBATCH --time=00:01:00
    #SBATCH --job-name=align_reads
    #SBATCH --mail-user=user@students.unibe.ch
    #SBATCH --mail-type=begin,end
    #SBATCH --output=/home/user/output_alignment_%j.o
    #SBATCH --error=/home/user/error_alignment_%j.e
    #SBATCH --array=0-2

    module add UHTS/Analysis/minimap2/2.17

    REFERENCE_DIR=/home/gvangeest/Courses/HPC_course/ecoli/reference
    READS_DIR=/home/gvangeest/Courses/HPC_course/ecoli/reads
    ALIGN_DIR=/home/gvangeest/Courses/HPC_course/ecoli/alignment

    mkdir $ALIGN_DIR

    cd $READS_DIR

    FILES=(*.fastq)

    minimap2 \
    -a \
    -x sr \
    $REFERENCE_DIR/ecoli-strK12-MG1655.fasta \
    ${FILES[$SLURM_ARRAY_TASK_ID]} \
    > $ALIGN_DIR/${FILES[$SLURM_ARRAY_TASK_ID]}.sam    
    ```
