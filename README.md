### Procedure followed to solve the lab2 problem.

Have done this assignment in ubuntu, so first of all, tried to install xv6 inside ubuntu
So in command prompt:
```bash
step 01: sudo apt-get update
step 02: sudo apt-get install build-essential
step 03: Sudo apt-get install gcc-multilib
step 04: sudo apt install qemu qemu-utils qemu-kvm virt-managerlibvirt-daemon-system libvirt clients bridge-utils
step 05: sudo apt-get install git-core
step 06: sudo git clone git://github.com/mit-pdos/xv6-public.git
step 07: cd xv6-public
step 08: sudo make
step 09: sudo make qemu-nox
```

Now exit from qemu, by pressing ctrl+a, then press x


Before proceed to the compile process, need to give permission to the xv6-public directory otherwise you will not able to run the code.<br/> 
For allowing the permission,run 

```bash
chmod -R a+x xv6-public
```

Now,again run qemu emulator in command prompt:
```bash
sudo make
sudo make qemu-nox
```
Then,have to check if testing system call working or not by giving the command:

`ls`

Outcome will be like:
```
init: starting sh
$ ls
.              1  1   512
..             1  1   512
README         2  2   2286
cat            2  3  16336
echo           2  4  15188
forktest       2  5   9504
grep           2  6  18556
init           2  7  15776
kill           2  8  15220
ln             2  9  15076
ls             2 10  17704
mkdir          2 11  15316
rm             2 12  15296
sh             2 13  27932
stressfs       2 14  16208
usertests      2 15  67316
wc             2 16  17072
zombie         2 17  14888
testTicket     2 18  15064
testProcInfo   2 19  15876
console        3 20      0
```

#### To complete the task i modified the following files:
`Makefile, param.h, proc.c, proc.h, syscall.c, syscall.h, sysproc.c, usys.s, user.h`
#### and created the following files:
`pstat.h, rand.h, rand.c, testProcInfo.c, testTicket.c`

To generate the `rand.c` file, i took the help from "http://www.math.sci.hiroshima-u.ac.jp/m-mat/MT/ARTICLES/mt.pdf"

and to add system call & user function in xv6, i followed previous lab lecture & this "https://www.youtube.com/watch?v=21SVYiKhcwM" tutorial.
 
Inside the `testTicket.c` file I called `setticket()` function and this `settickets()` is defined in `proc.c` file. this function is utilized for assigining tickets to each process.

& inside the `testProcInfo.c` file is for viewing the process status after lottery scheduling. by using the `getpinfo()` function which is also defined in `proc.c` file.

and to schedule using lottery process,i modified the `scheduler(void)` function inside `proc.c` file. So,now only lottery scheduling is possible, round robin scheduling isn't possible.

#### Now,to insert process & assign them pre-alloted tickets,run in qemu shell:
```bash
testTicket (any_integer)&;
```
or you can assign tickets to multiple processes at one command like this one:
```bash
testTicket 10&; testTicket 15&; testTicket 20&; testTicket 25&; testTicket 30&;
```

#### Now,if we want to see how the lottery scheduling is done to those assigned processes
run this command in qemu shell:
```bash
testProcInfo
```

A sample test case for previous input is given below:
```
------------ Initially assigned Tickets = 5 ----------

ProcessID	Runnable(0/1)	Tickets	      Ticks
1		       1		        5		    2
2		       1		        5		    2
16		       1		        5		    0
7		       1		        10		    72
9		       1		        15		    127
11		       1		        20		    151
13		       1		        25		    185
15		       1		        30		    229

------------------- Total Ticks = 768 -----------------
```
From the output we can observe that higher ticket holder processes are taking more ticks to execute.


Now to omit unnecessary file,run this command in cmd:
```bash
sudo make clean
```
### Major difficulties I have faced while solving this lab problem : 

While adding user function & system call, i had to modify in several files & it took much of my time to track & manage which file has been modified for which function. Also,errors like 'implicit function declaration' debugging was difficult. At the begining,how to fetch each process's status using pid pointer was also seemed difficult to me. And at first,i desgined my code will be interactive where user can assign priority & burst time to each processes,then my scheduler will do the lottery_schedule job. But not having input taking default functionalities in xv6' i had to struggle in planning & writing the code & i tried but couldn't implement dynamic priority to each process. Also,your mentioned `yield()` seems ambiguous to me




