# OPERATING SYSTEMS AND ITS TYPES



Q1) What is a Operating System?
\- controls execution of application program
\- act as interface between user and computer



Q2) What does an Operating System include?
-traffic controller
-scheduler
-memory management module
-I/O program
-file system



Q3) What is the use of an OS?
-hardware abstraction
-resource management



Q4) What are various types of OS?

1. **batch operating**

- doesn't interact with computer directly

- less idle time, multiple user, handles large work

- costly, hard to debug

- ex: payroll systems, bank statements

  

2. **time sharing**

- task is divided into time quantum's(tq)

- multitasking systems

- less chances of duplication

- low reliability

- ex: Unix

  

3. **distributed**

- loosely coupled systems

- processors differ in size and functions

- high exchange speed,reduces load on host,computing power is fast

- expensive, failure of main network

- ex: locus

  

4. **network**

- common server

- tightly coupled systems

- stable, enhanced security, updating is easy

- costly server, difficult to maintain

- ex: window server, Linux, mac-OS

  

5. **real time**

- time accurate (defense, nuclear power plants)
- types:
  1. hard real time:
     \* strict time constraint
     \* no virtual memory
  2.  soft real time:
     \* less strict time constraint
- task-shifting is easy, application focused, error free
- limited task, heavy resource requirements, complex algorithm based
- ex: weaponry system, air traffic control system



Q5) How MULTIPROGRAMMING HELPS IN REDUCING CPU IDLE TIME ?

in order to reduce the idle waiting time of the CPU (which happens when the job leaves the
job-pool (collection of all the jobs to be performed) and go for execution (I/O), CPU remains idle.

- with the help of multiprogramming as soon as the job leaves for I/O the OS chooses another job from the queue.
- so here the previous job do its I/O task and new job start doing it bound tasks reducing the CPU idle time



Q6) WHAT IS MULTIPROCESSING?

- Ability of the system to have more than one processors within the system
  multiprocessing is possible because the processors share the:
  i) computer bus
  ii) clock
  iii) memory
  iv) peripheral devices
- a dual core processor can perform two process at a time and a octa-core processor can do 8 process at a time



Q7) WHAT IS MULTITHREADING?

- it is a process in which we can allow different code segments to run concurrently within the context of the process.

- formally, it is the ability of a process to be used by many users at the single time and also manage different requests by the user

- for ex: using VLC media player we can use one thread to open VLC player another to run songs another thread to add music

- for ex : web server client architecture, here many requests by clients are handled by the server using threads

- multithreading increases the responsiveness of the system also it reduces the cost as we don't need to manage separate process for each users

  

Q4) what is meant by POST?

- **P**ower **O**n **S**elf **T**est.

- it is used to initialize the hardware devices of the computer

- how?

  in order to boot the system BIOS is used (now UEFI)

  - BIOS stand for (Basic Input Output System)
  - POST -> BIOS CHECK -> CMOS RAM ->CPU ->HARDWARE DEVICE ->SECONDARY STORAGE DEVICE
  - The POST first checks the BIOS and then tests the CMOS RAM. If there is no problem with this then POST continues to check the CPU, hardware devices such as the Video Card, the secondary storage devices such as the Hard Drive, Floppy Drives, Zip Drive or CD/DVD Drives. If some errors found then an error message is displayed on the screen or a number of beeps are heard. These beeps are known as POST beep codes. 
  - **LIMITATIONS OF BIOS**
  - can boot up to 2TB device
  - runs in 16 bit processor mode



**the complete process of loading with BIOS**
BIOS -> goes of POST -> check for MBR ->retrieve the BOOT-LOADER ->loads the system in RAM
and finally loads the OS

Q5) WHAT IS MBR?
      it is a program which runs when the computer is turning ON to find the OS.

