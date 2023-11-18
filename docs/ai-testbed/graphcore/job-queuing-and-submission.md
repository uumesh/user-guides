# Job Queueing and Submission

## Introduction

ALCF's Graphcore POD64 system uses PBS for job submission and queueing. Below are some of the important commands for using PBS. For more information see the User’s Guide (UG) section of the [BS Big Book](https://2021.help.altair.com/2021.1/PBSProfessional/PBS2021.1.pdf)


>**NOTE: Jobs that require IPUs will fail unless launched with `qsub`.**
>**NOTE: There is a single PBS scheduler for the Graphcore POD64.**

## qsub in interactive mode

The PBS command `qsub` can be used to start an interactive session allocated a specified number of IPUs. The interactive session will not inherit tle launching environment. Normal interactive shell startup will occur, e.g. .bashrc will be run for bash users.

Example:

```console
qsub -q workq -lwalltime=01:00:00 -lselect=1:ncpus=1+1:nipus=1 -IV
```

## qsub in batch mode

Alternatively, the PBS command `qsub` can be used to run scripts in batch mode, allocated a specified number of IPUs. Non-interactive shell startup will occur, e.g. .bashrc will be run for bash users, but in non-interactive mode, .bashrc typically exits before setting any user variables.

To do this, create a bash script (submit-mnist-poptorch-job.sh here as an example) with the commands that you want to execute.

```bash
#!/bin/sh

python mnist_poptorch.py
```

Then pass the bash script as an input to the `qsub` command as shown below, requesting the number of IPUs required:

```console
qsub -q workq -lwalltime=01:00:00 -lselect=1:ncpus=1+1:nipus=1 submit-mnist-poptorch-job.sh
```

<!--- See [DataParallel](DataParallel.md) for additional information. --->

## qstat

The `qstat` command provides information about jobs located in the (default) PBS scheduling queue.

```console
$ qstat -w
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
              2572       p64 Graphcor username  R       1:12      1 gc-poplar-02
```

## SInfo ??? TODO

TODO SInfo is used to view partition and node information for a system running Slurm.

```console
$ sinfo ? Is there any such thing for PBS? 
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
p64*         up   infinite      3   idle gc-poplar-[02-04]
```

For more information, see [SInfo](https://slurm.schedmd.com/sinfo.html).

## qdel

The qdel command deletes jobs in the order given.<br>
Get the job id using qstat -w

```bash
qdel job_id
```
