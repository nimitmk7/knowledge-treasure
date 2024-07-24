Mutex stands for Mutual Exclusion. 

Mutual Exclusion is a property of process synchronization that states that “no two processes can exist in the the critical section at any given point of time.”

 Mutex is mainly used to provide mutual exclusion to a specific portion of the code so that the process can execute and work with a particular section of the code at a particular time.

## Advantages
1. No race condition
2. Data consistency and integrity
3. Simple locking mechanism to be obtained by process before entering into critical section and to be released while leaving the critical section.

## Disadvantages
1. If after entering the critical section, the thread sleeps or gets pre-empted by a high-priority process, no other thread can enter into the critical section. This can lead to starvation.
2. When the previous thread leaves the critical section, then only other processes can enter into it, there is no other mechanism to lock or unlock the critical section.
3. Implementation of mutex can lead to busy waiting that leads to the wastage of the CPU cycle.

## Implementation
Java implements Mutex using the `synchronized` keyword. 

