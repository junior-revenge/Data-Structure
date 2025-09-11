
# 15.3 Dining Philosophers

## The Question
In the famous dining philosophers problem, a bunch of philosophers are
sitting around a circular table with one chopstick between each of them. A philosopher needs
both chopsticks to eat, and always picks up the left chopstick before the right one. A deadlock
could potentially occur if all the philosophers reached for the left chopstick at the same time. Using
threads and locks, implement a simulation of the dining philosophers problem that prevents deadlocks.

## Questions I Will Ask Before Going into Solution
- How one should represent Philosopher and Chopstick class. Whether Philosopher should extend Thread class directly.

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
한 명의 Philosopher가 개별 스레드로서 동작한다. 여기서 ReentrantLock()을 호출한 이유는, 저걸 쓰면 한 스레드 안에서 같은 락에 여러번 접근하는 건
허용해서 셀프 데드락은 막아주지만, 여러 스레드 즉 여러 철학자들이 접근하는 고전적인 데드락은 막지 않기 때문. 일단 데드락을 해결하지 않은 상태의 구현을 한 것.

데드락을 해결하는 방법 중 하나는, 아예 젓가락을 드는 행위 전체를 하나의 원자처럼 원자성을 띠게 하는 것. All-or-Nothing 방법이다. 오른쪽 젓가락을 못 들면 왼쪽 젓가락을 내려놓게 하는 것.


```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Chopstick {
    private Lock lock;

    public Chopstick() {
        lock = new ReentrantLock();
    }

    public boolean pickUp() {
        return lock.tryLock();
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
        if (pickUp()) {
            chew();
            putDown();
        }
    }

    public boolean pickUp() {
        if (!left.pickUp()) {
            return false;
        };
        if (!right.pickUp()) {
            left.putDown();
            return false;
        };
        return true;
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

그러나 이 방식은 모든 철학자 스레드가 동시에 왼쪽 젓가락을 들고 오른쪽 젓가락을 들려고 할 경우 작동하지 않을 것이다. 따라서 젓가락 자체에 우선순위를 두는 식으로 원천적으로 데드락을 차단할 수 있다. 이를 위해 젓가락에 라벨을 붙인다. 개별 철학자들은 언제나 더 라벨 상의 수가 작은 젓가락부터 집도록 한다. 이러면 모든 철학자가 동시에 왼쪽 젓가락을 들려고 시도해도 하나의 철학자는 오른쪽 젓가락부터 들도록 강제할 수 있다.

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Chopstick {
    private Lock lock;
    private int number;

    public Chopstick(int n) {
        lock = new ReentrantLock();
        this.number = n;
    }

    public boolean pickUp() {
        return lock.lock();
    }

    public void putDown() {
        lock.unlock();
    }

    public int getNumber() {
        return number;
    }
}

class Philosopher extends Thread {
    private int bites = 10;
    private Chopstick lower, higher;
    private int index;

    public Philosopher(int i, Chopstick left, Chopstick right) {
        index = i;
        if (left.getNumber() < right.getNumber()) {
            this.lower = left;
            this.higher = right;
        } else {
            this.lower = right;
            this.higher = left;
        }
    }

    public void eat() {
            pickUp();
            chew();
            putDown();
        }
    }

    public boolean pickUp() {
        lower.pickUp();
        higher.pickUp();

    public void chew() { }

    public void putDown() {
        higher.putDown();
        lower.putDown();
    }

    @Override
    public void run() {
        for (int i = 0; i < bites; i++) {
            eat();
        }
    }
}

```