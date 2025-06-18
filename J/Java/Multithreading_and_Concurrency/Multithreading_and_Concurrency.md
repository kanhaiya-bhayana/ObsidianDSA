#### Definition
- Allows a program to perform multiple task at the same time.
- Multiple threads share the same resource such as memory space but still can perform task independently.

#### Benefits and Challenges of Multithreading
- Improved performance by task parallelism
- Responsiveness
- Resource Sharing

#### Challenges
- Concurrency issue like deadlock, data inconsistency etc.
- Synchronized overhead.
- Testing and debugging is difficult.
### Process
> Process is an instance of a program that is getting executed.
> It has its own resource like memory, thread etc. OS allocate these resources to process when its created.
> 	
> 	Compilation (javac Test.java) --> generates bytecode that can be executed by JVM.
> 		
> 		Execution (java Test) --> at this point, JVM starts the new Process, here Test is the class which has `public static void main(String[] args)` method.

>One process can have multiple threads

---
### Thread
- Thread is known as lightweight process
	OR
- Smallest sequence of instructions that are executed by CPU independently.
- When a process is created, it start with 1 thread and that initial thread known as `main thread` and from that we can create multiple threads to perform task concurrently.


```java
public class MultiThreading{
	public static void main(String args[]){
		System.out.println("Thread name: " + Thread.currentThrea().getName());
	}
}
```

```lua
+-------------------------------+
|           Process2           |
|  +------------------------+  |
|  |     JVM Instance2      |  |
|  |  +------------------+  |  |
|  |  |   Heap Memory     |  |  |
|  |  +--------↑---------+  |  |
|  |           |            |  |
|  |  +--------------------+ |  |
|  |  |   Code Segment     | |  |
|  |  |   Data Segment     | |  |
|  |  +--------------------+ |  |
|  |  |  Thread1           | |  |
|  |  |  +----------+      | |  |
|  |  |  | Register |      | |  |
|  |  |  | Stack    |      | |  |
|  |  |  | Counter  |      | |  |
|  |  +--------------------+ |  |
|  |  |  Thread2           | |  |
|  |  |  +----------+      | |  |
|  |  |  | Register |      | |  |
|  |  |  | Stack    |      | |  |
|  |  |  | Counter  |      | |  |
|  |  +--------------------+ |  |
|  |  |  Thread3           | |  |
|  |  |  +----------+      | |  |
|  |  |  | Register |      | |  |
|  |  |  | Stack    |      | |  |
|  |  |  | Counter  |      | |  |
|  |  +--------------------+ |  |
|  +------------------------+  |
+-------------------------------+
```

#### Code Segment
- Contains the compiled BYTECODE (i.e., machine code) of the Java Program.
- Its read only.
- All threads within the same process, share the same code segment.

#### Data Segment
- Contains the GLOBAL and STATIC variables.
- All threads within the same process, share the same data segment.
- Threads can read and modify the same data.
- Synchronization is required between multiple threads.

#### Heap
- Objects created at runtime using `new` keyword are allocated in the heap.
- Heap is shared among all the threads of the same process. (but NOT WITHIN PROCESS)
- Threads can read and modify the heap data.
- Synchronization is required between multiple threads.

#### Stack
- Each thread has its own STACK.
- It manages, method calls, local variables.

#### Register
- When JIT(Just-in time) compiles converts the Bytecode into native machine code, its uses register to optimized the generated machine code.
- Also helps in Context Switching.
- Each thread has its own Register.

#### Counter
- Also known as Program Counter, it points to the instruction which is getting executed.
- Increments its counter after successfully execution of the instruction.


**ALL THESE ARE MANAGED BY THE JVM.**





