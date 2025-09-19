# Problem 15.2 Context Switch
How would you measure that time spent in a context switch?
#403, 415, 441

## Brute Force
* context switch is the process of storing the state of a process or thread, so that it can be restored and resume execution at a later point, and then restoring a different, previously saved, state.  -- wikipedia
* A context switch is the time spent switching between two processes. This happens when you bring one process into execution and swap out the existing process.
1. Try setting up two processes and have them pass a small amount of data back and forth. This will encourage the system to stop one process and bring the other one in.

## Solution.
This happens in multitasking. The operating system must bring the state information of waiting processes into memory and save the state information of the currently running process.
To solve this problem, we should record the timestamps of the last and first instruction of the swapping processes. **The context swtich time is the difference in the timestamps between the two preocesses.**  
How do we know when this swapping occurs? We can not, record the time stamp of every instruction in the process.
Another issue is thst swapping is governed by the scheduling algorithm of the operating system and there may be many kernel level threads which are also doing context switches. Other proccesses could be contending for the CPU or the kernel handling interrupts. 

In order to overcome these obstacles, we must first construct an environment such that after P1 executes, the task scheduler immediately selects P2 to run. This may be accomplished by constructing a data channel, such as a pipe, between P1 and P2 and vaving the two processes play a game of ping-pong with a data token. That is let's allow P1 to be the initial sender and P2 to be the receiver. Initially, P2 is blocked(sleeping) as it awaits the data token. When P1 executes it delivers the token over the data channel to P2 and immediately attemps to read a response token. However, since P2 hans not yet had a chance to run, no such token is available for P1 and the process is blocked. This relinquishes the CPU.  
A context switch results and the task scheduler must select another process to run. Since P2 is now in a ready to run state, it is a desirable candidate to be selected by the task sheduler for execution. When P2 runs, the roles of P1 and P2 are swapped. P2 is now acting as the sender P1 as the blocked receive. The game ends when P2 returns the token to P1.

---
The key is that the delivery of a data token induces a context switch. 
Let Td and Tr be the time it takes to deliver and receive a data token, repectively, and let Tc be the amount of time spent in a context switch. At step 2, P1 records the timestamp of the delivery of the token, and at step9, it records the timestamp of the response.   
The amount of time elapsed, T between these events may be expressed by `T = 2*( Td+Tc+Tr)`  
