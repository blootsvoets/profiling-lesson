---
layout: lesson
root: .
title: profiling applications
minutes: 15
---

## Learning Objectives 

When you start processing larger datasets on your *local* machine, you may discover that it is not going fast enough. And to speed the calculations up, you have to find out what the bottleneck is. The main bottlenecks are usually one or more of the following:

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

GUI `System Monitor`

#### Mac OS X

On Mac OS X the primary diagnostics tool is the Activity Monitor, found in `/Applications/Utilities/Activity Manager.app` (or use Spotlight and search Activity Monitor).

### Command line tools

General user tools that are installed on most Linux and Mac OS X systems are `top` and `ps`. The former is an interactive tool showing the current system status and all running processes. A version with more information is htop:

    htop

On OS X, you need to run `sudo htop`.

At the top, it will show you the processor activity of your system. Each processor is numbered. Below, under `Mem`, it shows how much memory is being used. Below that, in the `Swp` column.

## Approach

When your application is running, first look memory usage. Once about 80% of your memory is used, the system will remove caches and start doing operations from disk. If you run into this problem, a first step would be to see if your application can run with smaller chunks of data. If this is not possible, you can either get a machine with additional RAM memory or start a virtual machine on the Cloud with more memory than you have now. By using `htop` again, you can find out if the new machine does have enough memory. Alternatively, you can keep runninng application even after the memory of your machine is full, and see if it finishes. You can note the maximum memory usage in the VIRT column of the application. This number includes memory on disk, so it also gives relevant information once the system is using excessive memory.

The advantage of using a Cloud virtual machine is that it is very easy to experiment with different memory sizes.

Next, the application could simply be doing a lot of calculations using only one core. Since most machines have multiple cores, you are not using its full potential. In `htop`, you can see if your application is using multiple cores if the `%CPU` is more than 100%. Although somewhat counter-intuitive, the `%CPU` shows the percentage of *one* CPU being used. On a machine with 2 CPU cores, the optimal number would be 200%. The full potential of the system is being used when all the bars are full. Many demanding applications and algorithms allow you to specify the number of cores you want to use. Use the number of cores you have on your system. Please note that memory usage usually also increases when using multiple cores, so it would be wise to revisit the previous paragraph. On a virtual machine, you can increase the number of CPU cores on a virtual machine. Only do this as long as all the CPU bars at the top of `htop` are being filled. Ideally, if you are using `N` CPUs, using an additional CPU will decrease the time to completion `1 / (N + 1)`.

If memory is not fully being used, and the CPU is not fully being used, there may be a I/O bottleneck (input/output). These usually stem from hard drive speed or from networking. As you probably know, reading from or writing to hard drives is much slower than using computer memory. When an application needs to read a lot of data from file or from a database, the CPU will be waiting on data to arrive, and so will you. If this is your largest bottleneck, using a Cloud Virtual Machine will most likely not help you: there the problem will be worse because the file system is shared between multiple users. If the data is on an external hard drive, it may help to put it on a local hard drive while processing, and putting it back after the application is finished. If your hard drive is almost full, move or remove files until at most 80% of the disk is being used. If your machine does not have an SSD (Solid State Drive), arranging a machine with SSD or installing one would be a good investment.

<!-- If it is not installed, on Ubuntu or Debian Linux, install it with

    sudo apt-get install htop

On Fedora or CentOS use

    sudo yum install htop

Finally, on Mac OS X use [Homebrew](http://brew.sh):

    brew install htop -->

This command gives a lot of information in a single screen. The first line on the right shows the so-called 'load average'. This number is a combined measure of how much the system is being used. This makes it less useful for diagnosing your system. On the third line, you see the CPU usage. `%us` is the user usage. If this is

    ps aux

#### Super-user tools

The `htop` tool is similar to top:

    sudo yum install htop
    sudo apt-get install htop
    sudo htop

will tell you all the information of top with a graphical output. On OS X, 

    sudo iotop


## Exercises


The following R code snippets test your system dearly (save all work):

    x = data.frame(1:200000)
    for (i in 1:200000) { x[i] = 1:200000 }

Before you run, make sure you can cancel the command. Inspect what happens in the `top` command or your GUI.

    for (i in 1:20000000) { sqrt(nrand(0)) }

How many cores are being used?



REF
Brendan Gregg's slides  on inux-performance-analysis-and-tools
http://www.slideshare.net/brendangregg/linux-performance-analysis-and-tools?from_action=save
