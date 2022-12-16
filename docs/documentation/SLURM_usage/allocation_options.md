# Allocation options

Most options can be used for both `sbatch` and `srun`. Full documentation on `sbatch` can be found [here](https://slurm.schedmd.com/sbatch.html), and for `srun` [here](https://slurm.schedmd.com/srun.html).

| <div style="width:120px">Option</div>      | Description        | <div style="width:150px">Example</div>       | Default Value       |
|-------------|--------------------|---------------|---------------------|
|`--mail-user`  | User e-mail address. Used for e-mails on e.g. job failure | `--mail-user=user@students.unibe.ch` |  |
|`--mail-type`  | When to notify a job owner: _none, all, begin, end, fail, requeue, array_tasks_ | `--mail-type=end,fail` | |
|`--job-name`   | Specify a job name | `--job-name="Simple Matlab`" | |
|`--time`      | Expected runtime of the job. Format: `dd-hh:mm:ss` | `--time=12:00:00` <br> `--time=2-06:00:00`  | Partition-specific, see `scontrol show partition <partname>` |
|`--mem-per-cpu`| Memory required per allocated CPU in megabytes. Different units can be specified using the suffix [K\|M\|G] | `--mem-per-cpu=2G` | 2048 MB |
|`--tmp`        | Specify the amount of disk space that must be available on the compute node(s). The local scratch space for the job is referenced by the variable `SCRATCH`. Default units are megabytes. Different units can be specified using the suffix [K\|M\|G\|T]. | `--tmp=8G` <br>`--tmp=2048` | |
|`--ntasks`     | Number of tasks (processes). Used for MPI jobs and job steps that may run distributed on multiple compute nodes | `--ntasks=4 `| 1 or to match `--nodes`, `--tasks-per-node` if specified |
|`--nodes`      | Request a certain number of nodes | `--nodes=2` | 1 or to match `--ntasks`, `--tasks-per-node` if specified
|`--ntasks-per-node` | Specifies how many tasks will run on each allocated node. Meant to be used with `--nodes`. If used with the `--ntasks` option, the `--ntasks` option will take precedence and the `--ntasks-per-node` will be treated as a maximum count of tasks per node.| `--ntasks-per-node=2` | |
|`--cpus-per-task` | Number of CPUs per taks (threads). Used for shared memory jobs that run locally on a single compute node | `--cpus-per-task=4` | 1 |
|`--array`      | Submit an array job. Use `%` to specify the max number of tasks allowed to run concurrently. Only works in combination with `sbatch`| `--array=1,4,16-32:4` <br> -`-array=1-100%20` | |
|`--output`     | Redirect standard output. **All directories specified in the path must exist before the job starts!** | | By default stderr and stdout are connected to the same file `slurm-%j.out`, where `%j` is replaced with the job allocation number. |
|`--error`      | Redirect standard error. **All directories specified in the path must exist before the job starts!** | | By default stderr and stdout are connected to the same file `slurm-%j.out`, where `%j` is replaced with the job allocation number. |
|`--partition`  | The partition to use. | `--partition=pall` <br> `--partition=pshort` <br> `--partition=phighmem` | Default partition: pall |
|`--immediate`  | Only submit the job if all requested resources are immediately available | | |
|`--exclusive`  | Use the compute node(s) (with `sbatch`) or CPU(s) (with `srun`) exclusively. **CAUTION: use in combination with `sbatch` only if you know what your are doing** | | |
|`--test-only`  | Validate the batch script and return the estimated start time considering the current cluster state | | | 
|`--exclude`  | Explicitly exclude certain nodes from the resources granted to the job | `--exclude=binfservas01` | | 
|`--nodelist`  | Request a specific list of nodes. The job will contain _all_ of these hosts and possibly additional hosts as needed to satisfy resource requirements. | `--nodelist=binfservas01` | | 
