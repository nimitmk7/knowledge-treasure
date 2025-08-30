> **Definition:** Two or more threads hold some resources and wait forever for each other’s resources to be released.


# Coffman’s Conditions

> A **deadlock can occur** only when all **four conditions are true simultaneously** If you eliminate **any one**, deadlock is impossible.

## 1. Mutual Exclusion

At least one resource (e.g., a file, memory block, lock) is **non-shareable**, i.e., only one thread/process can use it at a time.

## 2. Hold and Wait
A thread holds one or more resources and is **waiting** to acquire additional resources currently held by other threads.

## 3. No pre-emption
Resources **cannot be forcibly taken** from a thread — they must be released **voluntarily**.

## 4. Circular Wait
There is a **closed chain** of threads, where each thread is waiting for a resource held by the next thread in the chain.

Thread A → waits for Lock B
Thread B → waits for Lock C
Thread C → waits for Lock A

This creates a **circular dependency** → no thread can proceed → 💥 deadlock.

# Preventing Deadlocks

|**Condition**|**Strategy to Break It**|
|---|---|
|Mutual Exclusion|Not always possible — some resources are inherently exclusive|
|Hold and Wait|Acquire all required resources at once|
|No Preemption|Use lock timeouts or interruptible locks|
|Circular Wait|Impose a strict lock acquisition order|


