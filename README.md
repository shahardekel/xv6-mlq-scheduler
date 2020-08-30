# xv6-mlq-scheduler
Implementing Multi Level Queue (MLQ) scheduler in xv6 operation system


The MLQ scheduler follow these rules:
• four priority levels, numbered from 3 (highest) down to 0 (lowest).
At creation, a process starts with priority 2.

• Whenever the xv6 timer tick occurs (by default this happens every 10 ms), the
highest priority process which is ready (‘RUNNABLE’) is scheduled to run. That is, this
is a preemptive scheduler.

• The highest priority ready process is scheduled to run whenever the previously
running process exits, sleeps, or otherwise ‘yields’ the CPU.

• The scheduler should schedule all the processes at each priority level, other than
level 0, in a round robin fashion. At level 0 it should use a FIFO scheduler.

• When a timer tick occurs, whichever process was currently using the CPU should be
considered to have used up an entire timer tick's worth of CPU, even if it did not
start at the previous tick (note that a timer tick is different than the time-slice).

Time-slices: 
Priority Timer ticks in the time slice:
priority   ticks
3          8
2          16
1          32
At level 0 it executes the process until completion or interrupted by a higher priority
process, or the process yields the CPU.
• If a process voluntarily relinquishes the CPU before its time-slice expires at a
particular priority level, its time-slice should not be reset; the next time that that
process is scheduled, it will continue to use the remainder of its existing time-slice at
that priority level.

I implemented additional system calls: 
1. int set_priority(int new_priority);
set the current process priority

2. int wait2(int *retime, int *rutime, int *stime, int* elapsed);
This function waits for a child process termination, and updates the following parameters:
retime: The aggregated number of clock ticks during which the process waited (was able to
run but did not get CPU)
rutime: The aggregated number of clock ticks during which the process was running
stime: The aggregated number of clock ticks during which the process was waiting for I/O
(was not able to run).
elapsed: number of ticks since process creation until death
I assume that the pointer parameters sent to the function are not NULL.
The function should return the pid of the child process or -1 if it fails for some reason.
