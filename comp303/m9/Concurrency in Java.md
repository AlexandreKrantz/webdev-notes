## Lesson Objectives
- Understand the concept of a Thread and its usefulness for programming; 
- Be able to write basic concurrent programs in Java; 
- Understand the causes of basic concurrency errors 
- Understand the mechanisms that help prevent the basic concurrency errors.

#### Why Concurrency?
- Use multiple processor cores
- Make UI more responsive
- Simplify the application model (decouple parts that don't need to be linked/synchronous).

## Theory: Process vs. Thread
- A **process** is a *self-contained* code execution environment (it has its own memory space, etc). 
	- Usually, the JVM runs on a single process. 
- A **thread** is a lightweight subprocess in the main process. 
	- Threads can be scheduled for execution independently by the OS. They also have their own program counter, stack, local variables, etc. 
	- However, they are still attached to the main process. They share memory, file handles, and security credentials with the main process. 

- **Heap memory** is used by all the parts of the application whereas **stack memory** is used only by one thread of execution. 
	- Whenever an object is created, it's always stored in the Heap space and stack memory contains the reference to it.
![[Pasted image 20230405105026.png]]

## Java Threads
### Object State Diagram
Possible states of a thread:
- New
- Runnable
- Timed waiting
- Waiting
- Blocked
- Terminated
![[Pasted image 20230407110236.png]]

### Create a Thread
Access the current thread object:
```java
public class MainThread {
    public static void main(String args[]) {
        Thread t = Thread.currentThread();
    }
}
```
- Each Thread is an instance of the `Thread` class. 

Create custom thread object:
```java
// create custom thread:
class MyThread extends Thread {
    public void run() {
        System.out.println("This is a new thread.");
    }
}
```
- `run` method is what gets executed when you start the thread. 

### Start a Thread
#### Method 1
- You can create custom threads by extending the `Thread` class. You just to need to override the `run()` method. 
```java
// create custom thread:
class MyThread extends Thread {
    public void run() {
        System.out.println("This is a new thread.");
    }
}

public class Main {
    public static void main(String[] args) {
	    // instantiate custom thread
        MyThread thread = new MyThread();
        
        // executes the run method of custom thread
        thread.start(); // in a separate execution thread
        
        System.out.println("This is the main thread.");
    }
}
```

#### Method 2
- Implement the `Runnable` interface and pass an instance of the implementing class to a new Thread object.

```java
class MyRun implements Runnable {
	long minPrime
	MyRun(long minPrime) { // Constructor
		this.min
	}
	@Override
	public void run() {
		// compute smth 
	}
}

public class MainThread {
	public static void main(String[] args) {
		// instantiate Runnable object
		Runnable p = new MyRun(143); 
		
		// pass the Runnable object as parameter into a new Thread
		new Thread(p).start(); 
	}
}
```

### Pause a Thread
Call the `.sleep(ms)` method on a `Thread` object. 
```java
// Example: pausing the main thread
try {
    Thread.currentThread().sleep(1000);
} catch (InterruptedException e) {
    // handle the exception
}
```

- *Sleeping a thread will give up its slot on the CPU.* 
	- A sleeping thread is in a "blocked" state. 
	- When sleep period is over, thread is in a "ready" state. 
- Other threads may be scheduled by the OS to run on the slot during its sleep time, and this could potentially block the thread from running for some additional time after its sleep period is over.
	- A thread in ready state may have to wait for other threads in the queue to finish. 

### Waiting for a Thread
- `myThread.join()` will block the current thread and execute `myThread` on its CPU slot instead. `myThread` is allowed to fully finish executing, at which point the current thread will resume executing again. 

The `InterruptedException` is a checked exception that can be thrown if the thread is interrupted while waiting, so you need to catch it in a try-catch block.
```java
try {
    myThread.join();
} catch (InterruptedException e) {
    // handle the exception
}
```

- Note that there are overloaded versions of this method. 

### Interrupting a Thread 
```java
myThread.interrupt(); // sets the interrupted property of the Thread object to true.
if (myThread.isInterrupted()) {
    // handle the interrupt
}
```

- When a thread is interrupted, it does not necessarily stop immediately. It's up to the thread to check its interrupted status periodically and decide how to handle the interrupt. Common ways to handle an interrupt include stopping the thread gracefully, cleaning up resources, and returning control to the caller.
	- Don't rely on the thread immediately stopping in your designs, or you might get concurrency bugs. 

## Thread Safety
- Ensure that shared variables or data structures are managed properly. 

### Immutable Objects
- Some objects are *stateless* (ie. no mutable instance variables). These objects are always thread safe. 
	- Same things as *immutable objects*. 
```java
public class Point {
    private final int x;
    private final int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() {
        return x;
    }
    
    public int getY() {
        return y;
    }
}
```
In this code, `Point` is a class that represents a point in two-dimensional space. The `x` and `y` variables are both `final`, meaning that they cannot be changed after they are initialized. This makes `Point` an immutable object, which is inherently thread-safe because it cannot be modified

### Atomic Operations
- Normal operations in Java are made up of multiple steps. 
- For example `value++` is actually a 3 part read-modify-write operation: 
	1. read the value
	2. add one to it
	3. write out the new value
- If statements also have multiple steps: 
	1. check if condition is true (and read variables) 
	2. execute inner instruction

- Compound operations can lead to "race conditions", where two thread access a shared resource concurrently. This leads to unexpected behavior because the execution order is not guaranteed.

- Atomic operations are operations that are executed as a single, indivisible unit.
	- This means that other instructions can't start executing before the operation is done. 

```java
import java.util.concurrent.atomic.AtomicInteger;

public class Counter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet();
    }
    
    public void decrement() {
        count.decrementAndGet();
    }
    
    public int getCount() {
        return count.get();
    }
}
```

In this code, `Counter` is a class that maintains a counter variable that can be accessed by multiple threads concurrently. The `count` variable is an `AtomicInteger`, which is a class that provides atomic operations on integers. This ensures that the `increment()` and `decrement()` operations are executed atomically, meaning that they are executed as a single, indivisible unit.

When you have non atomic operations next to atomic ones, you can get race conditions:
```java
class MutableName { 
	private AtomicReference aName; 
	private AtomicInteger aCount; 
	public void setName(String pName) 
	{ 
		aName.set(pName); // NON atomic operation !!
		aCount.incrementAndGet(); 
	} 
}
```

### Synchronization (aka Locking)
- Synchronization makes shared resources *serialized*; ie. only one thread can access the resource at a time. 
- However, it is important to note that using synchronization can lead to performance issues, because threads may block while waiting for the lock.

Example: `synchronized` keyword before a method means only one thread can access it at a time. This means it's okay to use non-atomic operations inside. 
```java
public class Counter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized void decrement() {
        count--;
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```
	- you can use the synchronized modifier to make any block thread safe

"Locking"
- When a thread calls the `increment()` method, we say it acquires the lock associated with the `Counter` object, increments the value, and releases the lock. 

#### Volatile Variables
Also, you may want to keep the get methods non synchronized so other methods can see the changes: 
```java
class MutableName {
	private volatile String aName;
	public void setName(String pName) {
		aName = pName;
	}
	public String getName() {
		return aName;
	}
}
```

The `volatile` keyword in Java is used to indicate that a variable's value may be modified by multiple threads at the same time, and that **changes made to the variable by one thread should be immediately visible to all other threads**.

Without the `volatile` keyword, it is possible for different threads to have different versions of the same variable in their local caches, which can lead to inconsistent behavior and hard-to-debug race conditions.

#### Intrinsic Locks
When a thread needs to access a synchronized block of code, it must first obtain the intrinsic lock associated with the object that is used as the monitor. Once a thread has acquired the lock, it is free to execute the synchronized block of code. Other threads that try to execute the same block of code will be blocked until the lock is released.

```java
public class Counter {
    private int count = 0;

    public void increment() {
        synchronized(this) {
            count++;
        }
    }

    public int getCount() {
        synchronized(this) {
            return count;
        }
    }
}
```

- Both methods are marked as `synchronized`, which means that only one thread can execute either of these methods at a time. 
- The `synchronized(this)` block is used to acquire the intrinsic lock associated with the `Counter` object.
	- If the intrinsic lock is available, it continues executing the synchronized method. Else, the thread is blocked until released by another thread. 

It's worth noting that in this example, we are using the `this` object as the lock associated with the `Counter` object. However, any Java object can be used as a lock. For example, we could define a separate `Lock` object and use that as the lock associated with the `Counter` object:
```java
public class Counter {
    private int count = 0;
    private final Object lock = new Object();

    public void increment() {
        synchronized(lock) {
            count++;
        }
    }

    public int getCount() {
        synchronized(lock) {
            return count;
        }
    }
}
```

**Lock-ordering deadlock** occurs when two or more threads attempt to acquire multiple locks in different orders, leading to a situation where each thread is holding one lock and waiting for the other lock to be released, resulting in a deadlock. If a program acquires only one lock at a time, then there is no possibility of acquiring locks in different orders, and therefore lock-ordering deadlock cannot occur.