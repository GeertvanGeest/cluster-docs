
## General status of nodes

In order to get the list of available nodes and partitions you can simply run the command:

```sh
sinfo
```

This will give you a summarized list of nodes and partitions, e.g.:

```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
pall*        up 28-00:00:0     21    mix binfservas[08-12,15,19-30,32-34]
pall*        up 28-00:00:0      1  alloc binfservas07
pall*        up 28-00:00:0      4   idle binfservas[13-14,16-17]
ptest        up 7-00:00:00      1   down binfservas99
pshort       up    3:00:00      2   idle binfservas[03,06]
phighmem     up 110-00:00:      1   idle binfservas03
pcmpg        up   infinite      1   idle binfservas18
pcoursea     up 28-00:00:0      1   idle binfservas36
pcourseb     up 28-00:00:0      1   idle binfservas35
```

Here you can find the different partitions, associated nodes and their time limits. 

If you are interested in the specifications of a certain node, e.g. one of the nodes in `pall`, you can use:

```sh
scontrol show node binfservas07
```

Which will result in:

```
NodeName=binfservas07 Arch=x86_64 CoresPerSocket=8 
   CPUAlloc=16 CPUTot=16 CPULoad=16.15
   AvailableFeatures=(null)
   ActiveFeatures=(null)
   Gres=(null)
   NodeAddr=binfservas07 NodeHostName=binfservas07 Version=20.11.8
   OS=Linux 3.10.0-1160.45.1.el7.x86_64 #1 SMP Wed Oct 13 17:20:51 UTC 2021 
   RealMemory=250000 AllocMem=120424 FreeMem=103877 Sockets=2 Boards=1
   State=ALLOCATED ThreadsPerCore=1 TmpDisk=6900000 Weight=1 Owner=N/A MCS_label=N/A
   Partitions=pall 
   BootTime=2021-10-25T15:48:07 SlurmdStartTime=2021-10-25T17:44:25
   CfgTRES=cpu=16,mem=250000M,billing=16
   AllocTRES=cpu=16,mem=120424M
   CapWatts=n/a
   CurrentWatts=0 AveWatts=0
   ExtSensorsJoules=n/s ExtSensorsWatts=0 ExtSensorsTemp=n/s
   Comment=(null)
```

This gives information on e.g. number of CPUs (`CPUTot=16`) and memory (`RealMemory=250000`), so 16 CPUs with 250 Gb of memory. From this overview you can also see how much memory and CPU are allocated on this node: `AllocMem=120424` shows that 120.4 Gb is allocated and `CPUAlloc=16` show that all 16 CPUs are allocated. 

## Status of a job

To get a simple overview of a job status, you can find it out based on the job ID:

```sh
squeue -j 7377455
```

Returning:

```
  JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
7377455      pall interact gvangees  R       0:13      1 binfservas10
```

Which gives you the partition, the status, time it has been running and the node. 

You can also get a list all jobs submitted by you (or another user) with:

```sh
squeue -A gvangeest
```

Returning:

``` 
  JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
7377456      pall interact gvangees  R       0:06      1 binfservas10
7377455      pall interact gvangees  R       2:00      1 binfservas10
```

After job completion, you migth be interested in how much CPU, time and memory your job actually took. For this, you could use:

```sh
seff 7317012
```

Returning:

```
Job ID: 7317012
Array Job ID: 7317010_6
Cluster: ibu_main
User/Group: gvangeest/gvangeest
State: COMPLETED (exit code 0)
Nodes: 1
Cores per node: 16
CPU Utilized: 16:08:54
CPU Efficiency: 4.32% of 15-13:31:28 core-walltime
Job Wall-clock time: 23:20:43
Memory Utilized: 32.29 GB
Memory Efficiency: 50.45% of 64.00 GB
```

Which gives you a quick overview on how efficient your job was on CPU and memory usage. 

In order to get some more specific characteristics of your job, you can use `sacct`, e.g.:

```sh
sacct -j 7317012 --format="NodeList,AllocCPUS,TotalCPU,ReqMem,MaxRSS,Timelimit,Elapsed"
```

Returning:

```
       NodeList  AllocCPUS   TotalCPU     ReqMem     MaxRSS  Timelimit    Elapsed 
--------------- ---------- ---------- ---------- ---------- ---------- ---------- 
   binfservas16         16   16:08:53        4Gc            3-00:00:00   23:20:43 
   binfservas16         16   16:08:53        4Gc  33855268K              23:20:43 
```

Here, the first line represents the allocation, whereas the second line represents the actual job step. 

If you want to see other parameters that `sacct` can provide, run `sacct -e`. You can conveniently pipe this to `grep`. E.g. to see all time related parameters, you can type:

```sh
sacct -e | grep Time
```

Returning:

```
Constraints         ConsumedEnergy      ConsumedEnergyRaw   CPUTime            
CPUTimeRAW          DBIndex             DerivedExitCode     Elapsed            
SystemCPU           SystemComment       Timelimit           TimelimitRaw 
```