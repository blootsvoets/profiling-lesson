---
layout: lesson
root: .
title: profiling applications
minutes: 15
---

## Learning Objectives 

When you start processing larger datasets on your *local* machine, you may discover that it is not going fast enough. And to speed the calculations up, you have to find out what the bottleneck is. The main bottlenecks are usually the following:

- memory size
- CPU core speed
- number of CPU cores
- hard drive space
- hard drive speed 

During this lesson we will use command-line tools to try to find out what the bottleneck is for your particular task. The bottlenecks are usually job-specific. The diagnostics usually tie in deeply into the computer architecture and so the lesson will require super-user privileges.


<!--
Make diagram to decide

large memory requirements: cloud
up to 16 CPUs: cloud or cluster
fast single core speed: maybe cloud or cluster - check!
hard drive space: external hard drive - cloud / cluster with large file system - backup!
hard drive -->


## Lesson 

###Graphical tools

#### Windows

On Windows, the built in Task Manager gives a lot of information on individual processes and on the total resource usage. You can find the Task Manager by right clicking on the Windows task bar and selecting the Task Manager. On the second tab, you can find individual process performance

Task Manager

#### Linux

GKrellM

#### Mac OS X

On Mac OS X the primary diagnostics tool is the Activity Monitor, found in `/Applications/Utilities/Activity Manager.app`.


### Command line tools

General user tools that are installed on most Linux and Mac OS X systems are `top` and `ps`. The former is an interactive tool showing the current system statys and all running processes. Try running top:

    top








    ps aux



#### Super-user tools

The `htop` tool is similar to top:

    sudo yum install htop
    sudo apt-get install htop
    sudo htop

will tell you all the information of top with a graphical output. On OS X, 

    sudo iotop


## Exercises


The following tests may test your system dearly (save all work):

    x = data.frame(1:200000)
    for (i in 1:200000) { x[i] = 1:200000 }

Before you run, make sure you can cancel the command. Inspect what happens in the `top` command or your GUI.

    for (i in 1:20000000) { sqrt(nrand(0)) }

Exercises