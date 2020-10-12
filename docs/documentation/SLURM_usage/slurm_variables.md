All variables can be found [here](https://slurm.schedmd.com/sbatch.html#lbAK). Below, the most frequently used variables:

| Variable               | Meaning                                                                                                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------|
| SLURM_ARRAY_TASK_COUNT | Total number of tasks in a job array.                                                                              |
| SLURM_ARRAY_TASK_ID    | Job array ID (index) number.                                                                                       |
| SLURM_ARRAY_TASK_MAX   | Job array's maximum ID (index) number.                                                                             |
| SLURM_ARRAY_TASK_MIN   | Job array's minimum ID (index) number.                                                                             |
| SLURM_ARRAY_TASK_STEP  | Job array's index step size.                                                                                       |
| SLURM_ARRAY_JOB_ID     | Job array's master job ID number.                                                                                  |
| SLURM_CLUSTER_NAME     | Name of the cluster on which the job is executing.                                                                 |
| SLURM_CPUS_ON_NODE     | Number of CPUS on the allocated node.                                                                              |
| SLURM_CPUS_PER_TASK    | Number of cpus requested per task. Only set if the --cpus-per-task option is specified.                            |
| SLURM_JOB_ACCOUNT      | Account name associated of the job allocation.                                                                     |
| SLURM_JOB_ID           | The ID of the job allocation.                                                                                      |
| SLURM_JOB_DEPENDENCY   | Set to value of the --dependency option.                                                                           |
| SLURM_JOB_NAME         | Name of the job.                                                                                                   |
| SLURM_JOB_NODELIST     | List of nodes allocated to the job.                                                                                |
| SLURM_JOB_NUM_NODES    | Total number of nodes in the job's resource allocation.                                                            |
| SLURM_JOB_PARTITION    | Name of the partition in which the job is running.                                                                 |
| SLURM_MEM_PER_CPU      | Same as --mem-per-cpu                                                                                              |
| SLURM_NODEID           | ID of the nodes allocated.                                                                                         |
| SLURM_NTASKS           | Same as -n, --ntasks                                                                                               |
| SLURM_SUBMIT_DIR       | The directory from which sbatch was invoked or, if applicable, the directory specified by the -D, --chdir option.  |
| SLURM_SUBMIT_HOST      | The hostname of the computer from which sbatch was invoked.                                                        |
| SLURM_TASKS_PER_NODE   | Number of tasks to be initiated on each node.                                                                      |
| SLURM_TASK_PID         | The process ID of the task being started.                                                                          |
| SLURMD_NODENAME        | Name of the node running the job script.                                                                           |
