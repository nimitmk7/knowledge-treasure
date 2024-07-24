Writing concurrent programs is easy, but things become tricky when we have to ensure correctness and optimality.

**Ensuring correctness**: Locking and atomic instructions
**Ensuring optimality**: Fairness, Efficient Logic

## Example
We count prime numbers till 100 million.

### Approach 1: Sequential
For each number, check if it is divisible by any of the numbers till its square root.

```Java
import java.lang.Math;

public class SequentialPrimeCounter {
    public static int checkPrime(long x) {
        if(x < 2) {
            return 0;
        }

        for(long i = 2; i <= Math.sqrt(x); i++) {
            if(x % i == 0)
                return 0;
        }
        return 1;
    }
    
    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        int primeCount = 0;
        for(long i=1; i < 100_000_000; i++) {
            primeCount += checkPrime(i);
        }
        long endTime = System.currentTimeMillis();
        long durationMs = endTime - startTime;

        int minutes = (int) (durationMs / 60000);
        int seconds = (int) ((durationMs % 60000) / 1000);
        System.out.println("Number of primes up to 100 million are" +":" + primeCount);
        System.out.printf("Time taken: %d minutes and %d seconds%n", minutes, seconds);
    }
}
```

Output: 
```
Number of primes up to 100 million are:5761455
Time taken: 0 minutes and 28 seconds
```
### Approach 2: Add Threads(Unfair)
10 threads, each handling an equal range of ~10 million.

```Java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

public class UnFairThreadsPrimeCounter {
    private static final long target = 100_000_000;
    private static final int CONCURRENCY = 10;
    private static final AtomicInteger totalPrimeNumbers = new AtomicInteger(0);

    private static void checkPrime(long x) {
        if ((x & 1) == 0) {
            return;
        }
        for (int i = 3; i <= Math.sqrt(x); i++) {
            if (x % i == 0) {
                return;
            }
        }
        totalPrimeNumbers.incrementAndGet();
    }

    private static void doBatch(String name, long nstart, long nend) {
        long start = System.nanoTime();
        for(long i = nstart; i < nend; i++) {
            checkPrime(i);
        }
        long duration = System.nanoTime() - start;
        System.out.printf("thread %s [%d, %d) completed in %.3f seconds%n", name, nstart, nend, duration / 1e9);
    }

    public static void main(String[] args) throws InterruptedException {
        long start = System.nanoTime();
        ExecutorService executor  = Executors.newFixedThreadPool(CONCURRENCY);

        long nstart = 3;
        long batchSize = target / Long.valueOf(CONCURRENCY);

        for(int i = 0; i < CONCURRENCY - 1; i++) {
            final long finalNstart = nstart;
            final int finalI = i;
            executor.submit(() -> doBatch(String.valueOf(finalI), finalNstart, finalNstart + batchSize));
            nstart += batchSize;
        }

        final long nstart_1 = nstart;
        executor.submit(() -> doBatch(String.valueOf(CONCURRENCY-1), nstart_1, target));
        executor.shutdown();
        executor.awaitTermination(10, TimeUnit.MINUTES);
        long duration = System.nanoTime() - start;

        System.out.printf("checking till %d found %d prime numbers. took %.3f seconds%n", 
        100_000_000, totalPrimeNumbers.get() + 1, duration / 1e9);
    }
}

```


We did speed up, but was it fair?
1. Smaller numbers can be checked quickly. For each number we check from 1 to sqrt(n).
2. More prime numbers in the smaller range.

Each thread is doing disproportionate amount of work, later threads have larger numbers to check. 

```
thread 0 [3, 10000003) completed in 2.571 seconds
thread 1 [10000003, 20000003) completed in 4.055 seconds
thread 2 [20000003, 30000003) completed in 4.859 seconds
thread 3 [30000003, 40000003) completed in 5.608 seconds
thread 4 [40000003, 50000003) completed in 5.874 seconds
thread 5 [50000003, 60000003) completed in 6.187 seconds
thread 6 [60000003, 70000003) completed in 6.439 seconds
thread 7 [70000003, 80000003) completed in 6.742 seconds
thread 8 [80000003, 90000003) completed in 6.926 seconds
thread 9 [90000003, 100000000) completed in 7.040 seconds

checking till 100000000 found 5761455 prime numbers. took 7.057 seconds
```

![[Pasted image 20240720232427.png]]

### Approach 3: Add Threads(Fair)
10 threads, each threads picks up the next unprocessed number and checks if it is prime.

All threads end at nearly the same time, and do nearly the same work, which is the maximum optimality condition.

```Java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

public class FairThreadsPrimeCounter {
    private static final long target = 100_000_000;
    private static final int CONCURRENCY = 10;
    private static final AtomicInteger totalPrimeNumbers = new AtomicInteger(0);
    private static final AtomicInteger currentNum = new AtomicInteger(2);

    private static void checkPrime(int x) {
        if ((x & 1) == 0) {
            return;
        }
        for (int i = 3; i <= Math.sqrt(x); i++) {
            if (x % i == 0) {
                return;
            }
        }
        totalPrimeNumbers.incrementAndGet();
    }

    private static void doWork(String name) {
        long start = System.nanoTime();
        while(true) {
            int x = currentNum.incrementAndGet();
            if (x > target) {
                break;
            }
            checkPrime(x);
        }
        long duration = System.nanoTime() - start;
        System.out.printf("thread %s completed in %.3f seconds%n", name, duration / 1e9);
    }

    public static void main(String[] args) throws InterruptedException {
        long start = System.nanoTime();
        ExecutorService executor = Executors.newFixedThreadPool(CONCURRENCY);

        for (int i = 0; i < CONCURRENCY; i++) {
            final int threadNum = i;
            executor.submit(() -> doWork(String.valueOf(threadNum)));
        }

        executor.shutdown();
        executor.awaitTermination(10, TimeUnit.MINUTES);

        long duration = System.nanoTime() - start;
        System.out.printf("checking till %d found %d prime numbers. took %.3f seconds%n", 
                          target, totalPrimeNumbers.get() + 1, duration / 1e9);
    }
}
```


## References

<iframe width="560" height="315" src="https://www.youtube.com/embed/2PjlaUnrAMQ?si=NRGKeswmct4rryDQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
