# Load Testing Guide and Example Code

## Purpose of Load Testing

### 1️⃣ Determine Maximum Operating Capacity of a Web Application

* Check how many concurrent users the system can handle reliably.
* Objectives:

  * Prevent service downtime
  * Estimate required infrastructure (server, network, DB)
  * Optimize costs (avoid over-provisioning or under-provisioning)

### 2️⃣ Identify Performance Bottlenecks

* Performance issues usually concentrate at one or two points.

  * Examples: DB queries, network bandwidth, CPU shortages
* By finding bottlenecks:

  * Clarify improvement points (e.g., add caching, optimize indexes, scale servers)
  * Improve perceived speed for users
  * Ensure scalability under increasing traffic

### 3️⃣ Verify Application Behavior Under Various Load Conditions

* Real traffic is unpredictable

  * Examples: shopping sale, ticketing, news spikes, game updates
* Through testing, check:

  * Can the system withstand sudden user surges?
  * Does the service fail gracefully when errors occur?

    * e.g., display proper messages
  * Ensure stable user experience (UX)

## Role of Load Testing Tools

* Simulate thousands to tens of thousands of virtual users connecting simultaneously
* Measure **response time, throughput, error rate, server resource usage (CPU, memory, etc.)**
* Identify performance bottlenecks and maximum concurrent users

## Load Testing Steps

1. Identify Critical Scenarios

* Determine which features or pages are most important for user experience

2. Define Metrics / Performance Goals

* Common metrics:

  * Response Time: How quickly the system responds
  * Throughput: Number of transactions processed per unit time
  * Resource Utilization: CPU, memory, disk, network usage
  * Maximum Load: Maximum number of concurrent users the system can handle reliably

3. Design Load Test

* Simulate target load and measure metrics
* Configure tests to mimic real user patterns

### Considerations

* Avoid overloading production servers
* Prefer a separate environment to prevent affecting DB or actual user data
* Network differences may cause deviations from actual environment

---

## Java Example: Load Testing with Monitoring

```java
import java.lang.management.ManagementFactory;
import java.lang.management.MemoryMXBean;
import java.lang.management.ThreadMXBean;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.concurrent.*;

public class LoadTestWithMonitoring {

    private static final String TARGET_URL = "http://localhost:8080/test";
    private static final int CONCURRENT_USERS = 100;
    private static final int REQUESTS_PER_USER = 10;

    public static void main(String[] args) throws InterruptedException {

        // ExecutorService manages a pool of threads in Java
        ExecutorService executor = Executors.newFixedThreadPool(CONCURRENT_USERS);
        // Set thread pool size to the number of concurrent users to test thread bottlenecks
        ConcurrentLinkedQueue<Long> responseTimes = new ConcurrentLinkedQueue<>();

        // Get JVM monitoring beans for CPU, memory, and threads
        ThreadMXBean threadMXBean = ManagementFactory.getThreadMXBean();
        MemoryMXBean memoryMXBean = ManagementFactory.getMemoryMXBean();

        for (int i = 0; i < CONCURRENT_USERS; i++) {
            // Each thread simulates a single user sending multiple requests
            executor.submit(() -> {
                for (int j = 0; j < REQUESTS_PER_USER; j++) {
                    long startTime = System.currentTimeMillis();
                    try {
                        URL url = new URL(TARGET_URL);
                        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
                        conn.setRequestMethod("GET");

                        int responseCode = conn.getResponseCode();

                        long duration = System.currentTimeMillis() - startTime;
                        responseTimes.add(duration);

                        // Simple server monitoring output
                        int threadCount = threadMXBean.getThreadCount();
                        long usedHeap = memoryMXBean.getHeapMemoryUsage().getUsed() / 1024 / 1024;

                        System.out.printf("Response: %d, Time: %dms, Threads: %d, Heap Used: %dMB%n",
                                responseCode, duration, threadCount, usedHeap);

                        conn.disconnect();
                    } catch (Exception e) {
                        System.err.println("Request failed: " + e.getMessage());
                    }
                }
            });
        }

        executor.shutdown();
        executor.awaitTermination(30, TimeUnit.MINUTES);

        double avg = responseTimes.stream().mapToLong(Long::longValue).average().orElse(0.0);
        System.out.println("Average Response Time: " + avg + "ms");
    }
}
```
