<div align="center">

# 🚀 easy_cluster

**Useful shell wrappers around common SLURM commands.**

![Bash](https://img.shields.io/badge/Bash-4EAA25?logo=gnubash&logoColor=white)
&nbsp;![Scheduler](https://img.shields.io/badge/scheduler-SLURM-2088FF)
&nbsp;![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

</div>

A small collection of shell wrappers around common SLURM commands, making it
easier to see who is using the cluster, how GPUs are allocated, and to manage
your own jobs.

All scripts are plain `bash` and rely on the standard SLURM CLI (`squeue`,
`sinfo`, `scancel`, `sprio`). Run any script with `-h` (where supported) to see
its full usage.

---

## 🧰 Commands &nbsp;<sub><i>(actively updating)</i></sub>

|  | Command | What it does |
| :-: | --- | --- |
| 📋 | [`myjobs`](myjobs) | Show your own jobs in the queue. |
| 👥 | [`whoson`](whoson) | Summarise running/pending jobs per user, optionally with GPU and node counts. |
| 📊 | [`gpu-usage`](gpu-usage) | Show aggregate GPU usage across the cluster (or selected nodes). |
| 🖥️ | [`gpu-usage-by-node`](gpu-usage-by-node) | Show GPU usage broken down per node. |
| 🟢 | [`free-gpus`](free-gpus) | List nodes that currently have free GPUs. |
| 🔪 | [`killmyjobs`](killmyjobs) | Cancel your jobs, with options to filter and exclude. |

> 💡 Click a command below to unfold its arguments.

<br>

<details>
<summary>📋 &nbsp;<kbd><b>myjobs</b></kbd> &nbsp;— Shows your own jobs in the queue (a thin wrapper around <code>squeue -u $USER</code>).</summary>

<br>

**Arguments**

Any arguments are passed straight through to `squeue`, so every `squeue` option
works. Some handy ones:

- `--start` — show the estimated start time for your pending jobs.
- `-t <state>` — filter by job state, e.g. `-t RUNNING` or `-t PENDING`.
- `-i <seconds>` — refresh the listing every N seconds (live view).

</details>

<details>
<summary>👥 &nbsp;<kbd><b>whoson</b></kbd> &nbsp;— Lists every user with queued or running jobs, showing a <code>running/pending/total</code> breakdown of their job count.</summary>

<br>

**Arguments**

- `-g` — additionally report, per user, the number of **nodes** and **GPUs**
  used (each as `running/pending/total`).

GPU counts are read from the job's TRES (`gres/gpu=N`), so they reflect the
total GPUs requested/allocated for each job regardless of how they were
requested (`--gpus`, `--gpus-per-node`, `--gpus-per-task`, `--gres`).

</details>

<details>
<summary>📊 &nbsp;<kbd><b>gpu-usage</b></kbd> &nbsp;— Prints aggregate GPU usage as <code>in_use</code>, <code>usable</code>, <code>total</code>, and <code>free</code> counts.</summary>

<br>

**Arguments**

- `-n <nodelist>` — restrict to specific nodes; accepts a comma-separated list
  (`charles01,charles03`) or SLURM's summarised form (`charles[01-06,11-19]`).
- `-p` — output raw CSV instead of the pretty table (the table is the default).
- `-h` — suppress the header row (only applies together with `-p`).

The number of free GPUs is `usable - in_use`.

</details>

<details>
<summary>🖥️ &nbsp;<kbd><b>gpu-usage-by-node</b></kbd> &nbsp;— Same GPU breakdown as <code>gpu-usage</code>, but with one row per node (prefixed with a timestamp and node name).</summary>

<br>

**Arguments**

- `-n <nodelist>` — restrict to specific nodes (same format as `gpu-usage`).
- `-p` — output raw CSV instead of the pretty table (the table is the default).
- `-H` / `-h` — force the header on / off (apply when using `-p`).

</details>

<details>
<summary>🟢 &nbsp;<kbd><b>free-gpus</b></kbd> &nbsp;— Lists each node that currently has at least one free GPU, with how many GPUs are free on it.</summary>

<br>

Takes no arguments. If every usable GPU is currently allocated, it instead tells
you the cluster is full and prints the current priority queue (`sprio -l`).

</details>

<details>
<summary>🔪 &nbsp;<kbd><b>killmyjobs</b></kbd> &nbsp;— Cancels your jobs; by default cancels <b>both</b> running and queued jobs.</summary>

<br>

**Arguments**

- `-g` — only cancel **running** jobs, leaving queued jobs in place.
- `-n <nodelist>` — only cancel jobs on the given nodes (same format as
  `gpu-usage`).
- `-e <JOBID>` — exclude a job from being cancelled; may be given multiple
  times.

</details>

---

## 📦 Installation

Open a terminal, and `cd` to `<PATH-ON-YOUR-CLUSTER>` then:

```bash
git clone https://github.com/NanboLi/easy_cluster.git
echo 'export PATH=<PATH-ON-YOUR-CLUSTER>/easy_cluster:$PATH' >> ~/.bashrc
source ~/.bashrc
```

You are ready to go! 🎉
