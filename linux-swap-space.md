# All about Linux swap space


Linux divides its physical RAM (random access memory) into chucks of memory called pages. Swapping is the process whereby a page of memory is copied to the preconfigured space on the hard disk, called swap space, to free up that page of memory. The combined sizes of the physical memory and the swap space is the amount of virtual memory available.

Swapping is necessary for two important reasons. First, when the system requires more memory than is physically available, the kernel swaps out less used pages and gives memory to the current application (process) that needs the memory immediately. Second, a significant number of the pages used by an application during its startup phase may only be used for initialization and then never used again. The system can swap out those pages and free the memory for other applications or even for the disk cache.

However, swapping does have a downside. Compared to memory, disks are very slow. Memory speeds can be measured in nanoseconds, while disks are measured in milliseconds, so accessing the disk can be tens of thousands times slower than accessing physical memory. The more swapping that occurs, the slower your system will be. Sometimes excessive swapping or thrashing occurs where a page is swapped out and then very soon swapped in and then swapped out again and so on. In such situations the system is struggling to find free memory and keep applications running at the same time. In this case only adding more RAM will help.

Linux has two forms of swap space: the swap partition and the swap file. The swap partition is an independent section of the hard disk used solely for swapping; no other files can reside there. The swap file is a special file in the filesystem that resides amongst your system and data files.

To see what swap space you have, use the command swapon -s. The output will look something like this:

    Filename  Type       Size       Used Priority

    /dev/sda5 partition  859436  0       -1

Each line lists a separate swap space being used by the system. Here, the ‘Type’ field indicates that this swap space is a partition rather than a file, and from ‘Filename’ we see that it is on the disk sda5. The ‘Size’ is listed in kilobytes, and the ‘Used’ field tells us how many kilobytes of swap space has been used (in this case none). ‘Priority’ tells Linux which swap space to use first. One great thing about the Linux swapping subsystem is that if you mount two (or more) swap spaces (preferably on two different devices) with the same priority, Linux will interleave its swapping activity between them, which can greatly increase swapping performance.

To add an extra swap partition to your system, you first need to prepare it. Step one is to ensure that the partition is marked as a swap partition and step two is to make the swap filesystem. To check that the partition is marked for swap, run as root:

    fdisk -l /dev/hdb

Replace /dev/hdb with the device of the hard disk on your system with the swap partition on it. You should see output that looks like this:

    Device Boot    Start      End           Blocks  Id      System

    /dev/hdb1       2328    2434    859446  82      Linux swap / Solaris

If the partition isn’t marked as swap you will need to alter it by running fdisk and using the ‘t’ menu option. Be careful when working with partitions — you don’t want to delete important partitions by mistake or change the id of your system partition to swap by mistake. All data on a swap partition will be lost, so double-check every change you make. Also note that Solaris uses the same ID as Linux swap space for its partitions, so be careful not to kill your Solaris partitions by mistake.

Once a partition is marked as swap, you need to prepare it using the mkswap (make swap) command as root:

    mkswap /dev/hdb1

If you see no errors, your swap space is ready to use. To activate it immediately, type:

    swapon /dev/hdb1

You can verify that it is being used by running swapon -s. To mount the swap space automatically at boot time, you must add an entry to the /etc/fstab file, which contains a list of filesystems and swap spaces that need to be mounted at boot up. The format of each line is:

Since swap space is a special type of filesystem, many of these parameters aren’t applicable. For swap space, add:

    /dev/hdb1       none    swap    sw      0       0

where /dev/hdb1 is the swap partition. It doesn’t have a specific mount point, hence _none_. It is of type _swap_ with options of _sw_, and the last two parameters aren’t used so they are entered as 0.

To check that your swap space is being automatically mounted without having to reboot, you can run the swapoff -a command (which turns off all swap spaces) and then swapon -a (which mounts all swap spaces listed in the /etc/fstab file) and then check it with swapon -s.

#### Swap file

As well as the swap partition, Linux also supports a swap file that you can create, prepare, and mount in a fashion similar to that of a swap partition. The advantage of swap files is that you don’t need to find an empty partition or repartition a disk to add additional swap space.

To create a swap file, use the dd command to create an empty file. To create a 1GB file, type:

    dd if=/dev/zero of=/swapfile bs=1024 count=1048576

/swapfile is the name of the swap file, and the count of 1048576 is the size in kilobytes (i.e. 1GB).

Prepare the swap file using mkswap just as you would a partition, but this time use the name of the swap file:

    mkswap /swapfile

And similarly, mount it using the swapon command: swapon /swapfile.

The /etc/fstab entry for a swap file would look like this:

    /swapfile       none    swap    sw      0       0

#### How big should my swap space be?

It is possible to run a Linux system without a swap space, and the system will run well if you have a large amount of memory — but if you run out of physical memory then the system will crash, as it has nothing else it can do, so it is advisable to have a swap space, especially since disk space is relatively cheap.

The key question is how much? Older versions of Unix-type operating systems (such as Sun OS and Ultrix) demanded a swap space of two to three times that of physical memory. Modern implementations (such as Linux) don’t require that much, but they can use it if you configure it. A rule of thumb is as follows: 1) for a desktop system, use a swap space of double system memory, as it will allow you to run a large number of applications (many of which may will be idle and easily swapped), making more RAM available for the active applications; 2) for a server, have a smaller amount of swap available (say half of physical memory) so that you have some flexibility for swapping when needed, but monitor the amount of swap space used and upgrade your RAM if necessary; 3) for older desktop machines (with say only 128MB), use as much swap space as you can spare, even up to 1GB.

The Linux 2.6 kernel added a new kernel parameter called _swappiness_ to let administrators tweak the way Linux swaps. It is a number from 0 to 100. In essence, higher values lead to more pages being swapped, and lower values lead to more applications being kept in memory, even if they are idle. Kernel maintainer Andrew Morton has said that he runs his desktop machines with a swappiness of 100, stating that “My point is that decreasing the tendency of the kernel to swap stuff out is wrong. You really don’t want hundreds of megabytes of BloatyApp’s untouched memory floating about in the machine. Get it out on the disk, use the memory for something useful.”

One downside to Morton’s idea is that if memory is swapped out too quickly then application response time drops, because when the application’s window is clicked the system has to swap the application back into memory, which will make it feel slow.

The default value for swappiness is 60. You can alter it temporarily (until you next reboot) by typing as root:

    echo 50 > /proc/sys/vm/swappiness

If you want to alter it permanently then you need to change the _vm.swappiness_ parameter in the /etc/sysctl.conf file.

#### Conclusion

Managing swap space is an essential aspect of system administration. With good planning and proper use swapping can provide many benefits. Don’t be afraid to experiment, and always monitor your system to ensure you are getting the results you need.