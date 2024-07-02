## What is a Program ?

A program is an executable file which contains the code or a set of instructions, that is stored as a file on a disk. 
![[Pasted image 20240701195747.png | 400]]

![[Pasted image 20240701195822.png | 500]]

## What is a Process ?
When the code in a program is **loaded into main memory** and **executed by the processor**, it becomes a process. 

![[Pasted image 20240701195902.png | 500]]

An active process also includes the resources the program needs to run. These resources include: 
1. CPU resources 
2. RAM resources
3. I/O resources
and are managed by the OS.

Examples of resources are: 
1. Processor registers
2. Program counters
3. Stack pointers
4. Memory Pages, etc.

### Memory Management

Each process has its own memory address space. One process can’t corrupt the memory space of another process. So when one process malfunctions, other processes can keep running.

![[Pasted image 20240701200243.png | 500]]

## What is a thread ?
A thread is the unit of execution within a process. A process has at least one thread, which is the main thread. Typically, a process has many threads.
![[Pasted image 20240701200741.png | 300]]

Each thread has its own stack and registers. But threads within a process share a memory address space. 

![[Pasted image 20240701212025.png]]

However, one misbehaving thread can bring down the entire process. 

## How does OS run the thread/process on CPU ?

![[Pasted image 20240701212146.png]]

This is handled by context switching. During a context switch, one process is switched out of CPU so that another process can run.

![[Pasted image 20240701212241.png]]

- The OS stores the state of the current running process so the process can be restored and resume execution at a later point. 
- It then restores the previously saved states of a different process and resumes execution for that process. 

![[Pasted image 20240701213426.png]]
![[Pasted image 20240701213741.png | 300]]

Context switching is expensive, it involves saving and loading of registers, switching out memory pages, and updating various kernel data structures. 

Switching execution between threads also require context switching. But it is generally faster to switch context between threads than between processes. 

That is because there are fewer threads to track, and more importantly, since threads share the same memory address space, there is no need to switch out virtual memory pages, which is one of the most expensive operations during a context switch. 

## References

<iframe width="560" height="315" src="https://www.youtube.com/embed/4rLW7zg21gI?si=EwaeS6HzplSD_Bas" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>









