
# 15.3 Dining Philosophers

## The Question
In the famous dining philosophers problem, a bunch of philosophers are
sitting around a circular table with one chopstick between each of them. A philosopher needs
both chopsticks to eat, and always picks up the left chopstick before the right one. A deadlock
could potentially occur if all the philosophers reached for the left chopstick at the same time. Using
threads and locks, implement a simulation of the dining philosophers problem that prevents deadlocks.

## Questions I Will Ask Before Going into Solution
- How e

## Approach
일단 데드락 문제를 해결하지 않은 상태의 코드:
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Chopstick {
    private Lock lock;

    public Chopstick() {
        lock = new ReentrantLock();
    }

    public void pickUp() {
        lock.lock();
    }

    public void putDown() {
        lock.unlock();
    }
}

class Philosopher extends Thread {
    private int bites = 10;
    private Chopstick left, right;

    public Philosopher(Chopstick left, Chopstick right) {
        this.left = left;
        this.right = right;
    }

    public void eat() {
        pickUp();
        chew();
        putDown();
    }

    public void pickUp() {
        left.pickUp();
        right.pickUp();
    }

    public void chew() { }

    public void putDown() {
        right.putDown();
        left.putDown();
    }

    @Override
    public void run() {
        for (int i = 0; i < bites; i++) {
            eat();
        }
    }
}

```