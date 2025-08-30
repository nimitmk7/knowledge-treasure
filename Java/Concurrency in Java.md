# Creating Threads

## Extending the Thread class

```Java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running...");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start(); // Important: use start(), NOT run()
    }
}
```
- start() ➔ creates a new thread and calls run().
- **Never call run() manually** ➔ it will just run like a normal method.
## Implementing the Runnable interface

```Java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Runnable running...");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start();
    }
}
```

> 🔥 **Runnable is better** than extending Thread ➔ because Java supports **single inheritance**, and using Runnable is more flexible.

# Thread Lifecycle

|**Stage**|**Description**|
|---|---|
|New|Thread is created but not started|
|Runnable|Thread is ready to run|
|Running|Thread is running|
|Blocked/Waiting|Waiting for a resource|
|Terminated|Thread has finished execution|
# Key methods in Thread Class

| **Method**  | **Purpose**                            |
| ----------- | -------------------------------------- |
| start()     | Start the thread                       |
| run()       | Code inside thread                     |
| sleep(ms)   | Pause thread for some milliseconds     |
| join()      | Wait for another thread to finish      |
| interrupt() | Interrupt a sleeping or waiting thread |
| isAlive()   | Check if a thread is still running     |

# Synchronization
Threads share memory ➔ two threads can **corrupt shared data** if they access it simultaneously.

**Solution**: Use synchronized to **lock** access.

```Java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

# ExecutorService
Instead of creating threads manually, Java provides **Executors** ➔ thread pools, task management.

```Java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        ExecutorService service = Executors.newFixedThreadPool(2);
        service.execute(() -> System.out.println("Task 1"));
        service.execute(() -> System.out.println("Task 2"));
        service.shutdown(); // Don't forget to shut it down
    }
}
```

- **Thread pools** reuse threads instead of creating new ones every time.
- **execute()** runs a task.

# Example

Build a simple multithreaded app** — a **Banking System** where **multiple threads deposit and withdraw** from the same account.

This is one of the **best real-world exercises** to _truly_ understand **threads + synchronization**.

---

##  **🏦 Problem Setup: Bank Account Simulation**

- We have a BankAccount class.
    
- Multiple threads will try to **deposit** and **withdraw** money **at the same time**.
    
- We must **synchronize** access to the shared balance to avoid corruption (like negative balance).
    

---

#### **✅ Step 1: Create** **BankAccount** **class**

```Java
public class BankAccount {
    private int balance = 1000; // Initial balance

    // Synchronized method to deposit money
    public synchronized void deposit(int amount) {
        balance += amount;
        System.out.println(Thread.currentThread().getName() + " deposited " + amount + ", Balance: " + balance);
    }

    // Synchronized method to withdraw money
    public synchronized void withdraw(int amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println(Thread.currentThread().getName() + " withdrew " + amount + ", Balance: " + balance);
        } else {
            System.out.println(Thread.currentThread().getName() + " tried to withdraw " + amount + " but insufficient balance: " + balance);
        }
    }

    public int getBalance() {
        return balance;
    }
}
```

✅ synchronized ensures **only one thread** modifies balance at a time.

---

### **✅ Step 2: Create Worker Threads**

Each thread will **either deposit** or **withdraw**:

```Java
public class BankingTask implements Runnable {
    private BankAccount account;
    private boolean depositTask;

    public BankingTask(BankAccount account, boolean depositTask) {
        this.account = account;
        this.depositTask = depositTask;
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) { // do 5 transactions
            if (depositTask) {
                account.deposit(100);
            } else {
                account.withdraw(50);
            }

            try {
                Thread.sleep(100); // sleep to simulate delay
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

✅ Runnable is used here ➔ better for flexibility and executor services later.

---

### **✅ Step 3: Main Program to Run It All**

```Java
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        // Create two deposit threads and two withdrawal threads
        Thread t1 = new Thread(new BankingTask(account, true), "Depositor-1");
        Thread t2 = new Thread(new BankingTask(account, false), "Withdrawer-1");
        Thread t3 = new Thread(new BankingTask(account, true), "Depositor-2");
        Thread t4 = new Thread(new BankingTask(account, false), "Withdrawer-2");

        // Start all threads
        t1.start();
        t2.start();
        t3.start();
        t4.start();
    }
}
```


✅ Threads will **start concurrently** and **access the shared BankAccount** safely.

### **✅ Step 4: Using ExecutorService Instead**

```Java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        // Create a thread pool with 4 threads
        ExecutorService executor = Executors.newFixedThreadPool(4);

        // Submit tasks to the executor
        executor.submit(new BankingTask(account, true));  // Depositor-1
        executor.submit(new BankingTask(account, false)); // Withdrawer-1
        executor.submit(new BankingTask(account, true));  // Depositor-2
        executor.submit(new BankingTask(account, false)); // Withdrawer-2

        // Important: shut down the executor after tasks are submitted
        executor.shutdown();
    }
}
```



---
### **🛡️ What Happens Internally**

- Depositor-1 and Withdrawer-1 both **try to modify balance**.
- **Without synchronization**, you could get **race conditions** (wrong balances, negative money!).
- **With synchronization**, only one thread at a time can modify balance, making it **safe and correct**.
    


