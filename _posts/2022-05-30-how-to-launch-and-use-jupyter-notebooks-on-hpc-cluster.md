---
layout: post
title: How to launch and use Jupyter Notebooks on HPC Cluster
author: Felix
comments: false
tags: fast.ai
excerpt_separator: "<!--more-->"
sticky: false
hidden: false
featureImage: ''

---
\# How to launch and use Jupyter Notebooks on HPC Cluster

This short guide shows you how to use Jupyter launched on

HPC exclusive node.

Such jupyter notebook could be very useful for debugging or testing

of code that demands a lot of computational power. <!--more-->

\## Running jupyter as SBATCH job:

1\. On the cluster have a script, e.g. run_jupyter.sh ,  with the following code:

\`\`\`

    #!/bin/bash -l
    
    #SBATCH --partition=<zmmk-exclusive or excellence-exclusive>
    
    #SBATCH --job-name=<job name>
    
    #SBATCH --nodes=<nr of nodes>
    
    #SBATCH --gres=gpu:<nr of gpus>
    
    #SBATCH --mem=<number of gb>
    
    #SBATCH --time=<time in minutes>
    
    #SBATCH --output=<file to log standard output, important to see the notebook server URI!!!!>
    
    #Go to the folder you wanna run jupyter in
    
    cd $HOME
    
    #Pick a port
    
    PORT=<port nr>
    
    #Forward port to cheops1 cluster
    
    ssh -N -f -R $PORT:localhost:$PORT cheops1
    
    #run conda
    
    conda activate <env name>
    
    #Start the notebook
    
    jupyter notebook --no-browser --port=$PORT

\`\`\`

2\. Now everytime you want to run the remote jupyter server, from your local computer do:

\`\`\`

    $ ssh -L <local port number e.g.: 8080>:localhost:<the port number you had on the script> user@cheops1.rrz.uni-koeln.de
    
    \[user@cheops1 \~\]$ sbatch run_jupyter.sh

\`\`\`

And access the jupyter in your web browser by typing the URI printed in the output file of your sbatch job \\

Note: the port of the URI should be your local port (8080 in this example), not the port on cheops

\## Running jupyter as SRUN job:

1\. Connect via SSH to your HPC login node (cheops0)

2\. You can skip this step if you have already jupyter installed.

Install jupyter in your HPC account:\\

$ module load python/3.6.8\\

$ python3 -m pip install --user jupyter

3\. Start screen session:\\

$ screen

4\. Launch bash session on zmmk-exclusive node using srun command:\\

$ srun -p zmmk-exclusive --time=\\<time in minutes> --gres=gpu:\\<number of gpus> --mem=\\<number of gigabytes>gb --cpus-per-task=\\<number of CPUs> --pty bash

\*Read and remember node number in shell prompt. You will use it soon for port forwarding.*

As zmmk-exclusive nodes do not have direct access to the web so you should forward

SSH traffic first to your login node which has direct internet connection.

5\. Start a jupyter notebook without opening a browser:\\

$ jupyter notebook --no-browser --port=8080

N.B. You could choose different port instead of 8080 but pay attention to its number.

\*Save a URL link with token inside because it will be requested for accessing Jupyter

remotely.*

6\. Detach screen display by key combination:\\

CTRL+A then D\\

You should be now back on your login node (cheops0).

7\. Start another screen session:\\

$ screen

8\. Start port forwarding:\\

$ ssh -L 8080:localhost:8080 <USER_NAME>@<NODE_NAME>\\

Where <NODE_NAME> should be in format cheops<some_number>

the number should have been taken in step #4.

9\. Detach screen session (CTRL+A then D).

10\. Go to the terminal on your local computer.

11\. Now you connect to HPC via SSH with port forwarding.

There're two ways to do this: by ordinary ssh client (B) or

by using autossh (A).

For convenience and stability of work I advise you to use

identity file instead of the password and to have autossh preinstalled

as it provides higher stability than ordinary ssh client being able

to reconnect if there's an interruption in internet connection.

You can install autossh easily on Linux or Mac OS simply:\\

for Linux\\

$ sudo apt install autossh\\

or for Mac OS\\

$ brew install autossh

For Windows there should be some way to install too.

A. If you have autossh installed run the command in shell:\\

$ autossh -N -M 0 -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3" -L 8080:localhost:8080 -i <PATH TO IDENTITY FILE> <USER_NAME>@cheops.rrz.uni-koeln.de

B. Otherwise use this:\\

$ ssh  -L 8080:localhost:8080 -i <PATH_TO_IDENTITY_FILE> <USER_NAME>@cheops.rrz.uni-koeln.de

12\. If you followed all steps above correctly then you should

be able now to access Jupyter notebook running on HPC exclusive node.

Open your favorite browser and paste to address field the URL of running jupyter session

which should have been copied on step #5.