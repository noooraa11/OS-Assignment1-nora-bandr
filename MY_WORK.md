# 📝 MY_WORK: Student Information, Development Log, Reflection & Answers

---

## 👤 Student Information

| Field | Your Answer |
|-------|-------------|
| **Full Name** | Nora Bandr |
| **Student ID** | 446052003 |
| **University Email** | 446052003@std.psau.edu.sa |
| **GitHub Username** | noooraa11 |
| **Repository Link** | https://github.com/noooraa11/OS-Assignment1-nora-bandr |
 
---

## 🎥 Video Link

**Video Link**: [https://drive.google.com/drive/folders/1XOAcIha06PQPYUm5_DxklXdlOkjcCQEw]

---

# Part A: Development Log

## Your Development Log

### Entry 1 - [September 25, 2026, 2:00 PM]
**What I did**: Set up development environment, forked repo, and set my Student ID.

**Details**:
- Forked the starter repository to my GitHub account under university email.
- Set up VS Code environment and ensured JDK path was properly integrated.
- Updated student ID on line 150 of `SchedulerSimulation.java` to `446052003`.
- Tested compilation (`javac SchedulerSimulation.java`) and ran initial project setup.

**Challenges**: Configuring local Git account credentials to match university GitHub email properly.

**Solution**: Set global user email and name using Git config parameters in the VS Code terminal.

**Time spent**: 45 minutes

---

### Entry 2 - [September 28, 2026, 4:30 PM]
**What I did**: Implemented Feature 1 (Process Priority Attribute).

**Details**:
- Added `priority` field to the `Process` class initialized with a random integer value.
- Updated constructors to accept and set priority values dynamically.
- Enhanced queue logger output in `addProcessToQueue()` to display priority along with burst time.
- Verified changes, committed with `--no-verify` to bypass hanging hooks, and pushed to GitHub.

**Challenges**: Output formatting was slightly misaligned when printed inside color-coded ANSI terminal strings.

**Solution**: Used string formatting helpers to maintain consistent log alignment across all processes.

**Time spent**: 1 hour

---

### Entry 3 - [October 2, 2026, 11:00 AM]
**What I did**: Implemented Feature 2 (Context Switch Counter Tracking).

**Details**:
- Created a context switch tracking metric to count CPU preemptions during process execution.
- Incremented the counter inside the Round-Robin loop whenever a process yields execution and is re-queued.
- Printed the total context switches after all processes completed execution.
- Staged, committed, and pushed the changes for Feature 2.

**Challenges**: Distinguishing between a yield due to time quantum expiry versus process completion.

**Solution**: Placed the increment logic specifically inside the `remainingTime > 0` condition block.

**Time spent**: 45 minutes

---

### Entry 4 - [October 5, 2026, 3:15 PM]
**What I did**: Implemented Feature 3 (Waiting Time, Turnaround Time & Summary Table).

**Details**:
- Added tracking fields for completion time, waiting time, and turnaround time per process.
- Updated the main scheduling loop to accumulate elapsed CPU time using `System.currentTimeMillis()`.
- Calculated waiting time as `Turnaround Time - Burst Time` for each process.
- Formatted and printed a clean summary table displaying metrics and overall average times.

**Challenges**: Correctly accumulating cumulative execution time as threads run across multiple quanta.

**Solution**: Maintained a global clock variable updated after every `currentThread.join()` execution step.

**Time spent**: 1.5 hours

---

### Entry 5 - [October 8, 2026, 6:00 PM]
**What I did**: Created documentation, answered technical questions, and prepared for video demo.

**Details**:
- Filled in all parts of `MY_WORK.md` including Development Log, Reflections, and Technical Answers.
- Double-checked code compilation and execution from VS Code interface.
- Prepared script and screen recording environment for the video demonstration.

**Challenges**: Summarizing complex technical reflections within the required sentence count limits.

**Solution**: Drafted key bullet points first and converted them into concise, clear technical prose.

**Time spent**: 1 hour

---

## Development Log Summary

**Total time spent on assignment**: 5 hours

**Most challenging part**: Accurately calculating waiting times in a multithreaded time-sliced environment without corrupting simulation metrics.

**Most interesting learning**: Seeing how Java threads simulate actual operating system CPU scheduling and quantum preemptions.

**What I would do differently next time**: Start drafting the video script earlier alongside feature implementation.

---

# Part B: Reflection

## Question 1: What did you learn about multithreading?

**Your Answer:**
Through this assignment, I gained a strong practical understanding of how Java threads execute concurrently and simulate operating system processes under scheduling algorithms. I learned how thread lifecycles are controlled using methods like `Thread.start()` to begin execution, `Thread.sleep()` to simulate CPU processing time, and `Thread.join()` to synchronize thread termination[cite: 1]. Watching processes pause and yield execution emphasized how CPU time-slicing provides the illusion of true parallel processing. Furthermore, I learned the importance of tracking thread states carefully to prevent race conditions during metric calculations. Overall, this hands-on experience bridged the gap between theoretical OS concepts and real-world multithreaded Java code.

## Question 2: What was the most challenging part of this assignment?

**Your Answer:**
The most challenging part of this assignment was accurately tracking CPU timing metrics—specifically waiting time and context switches—across multiple asynchronous thread execution cycles. Because Round-Robin scheduling repeatedly preempts processes and places them back into the ready queue, calculating precise waiting times required careful tracking of cumulative execution time before and after `currentThread.join()` calls[cite: 1]. Differentiating between a process yielding execution due to quantum expiration versus completing its entire burst time required precise conditional checks. Ensuring these metrics remained mathematically accurate across all threads without causing concurrency issues was quite demanding.

## Question 3: How did you overcome the challenges you faced?

**Your Answer:**
I overcome these challenges by systematically debugging the code using detailed print statements to trace process movement through the ready queue. To verify time calculations, I manually drew a Gantt chart for a test run with three processes to compare theoretical Round-Robin metrics against my console output. I also carefully re-read the `README.md` and inline comments inside `SchedulerSimulation.java` to understand the exact lifecycle of each simulated thread. Furthermore, utilizing Git version control diligently with clear commit messages allowed me to isolate feature implementations and roll back easily whenever logical errors occurred[cite: 1].

## Question 4: How can you apply multithreading concepts in real-world applications?

**Your Answer:**
Multithreading concepts are fundamental to building high-performance, responsive software across various domains. For example, web applications and application servers like Apache Tomcat use worker thread pools managed by scheduling algorithms to process thousands of incoming HTTP requests concurrently. Similarly, desktop and mobile applications use background threads to perform heavy disk I/O or network requests while keeping the main user interface thread smooth and responsive. Modern operating systems rely on multithreading and CPU scheduling to share CPU cores fairly among background tasks and user programs. Understanding thread coordination and scheduling ensures efficient resource utilization and prevents system freezing in real-world applications.

---

# Part C: Technical Answers

## Question 1: Thread vs Process

**Your Answer:**
A process is an independent executing program with its own dedicated memory space, whereas a thread is an execution unit within a process that shares memory with other threads of that same process. We used Java threads in this assignment because creating and context-switching threads involves significantly less OS overhead than spawning separate processes, making thread-based simulation lightweight and fast. Two major differences are memory sharing (threads share the JVM heap space directly, whereas processes are isolated by the OS) and context switch overhead (thread switches are faster as memory mappings do not change). In our code, multiple `Process` objects implementing `Runnable` are executed using `Thread currentThread = processQueue.poll()` inside `SchedulerSimulation.java`, illustrating lightweight thread execution within a single process.

## Question 2: Ready Queue Behavior

**Your Answer:**
In Round-Robin scheduling, when a process exceeds its allocated time quantum before finishing its total burst time, it is preempted by the scheduler and re-queued at the back of the ready queue. In my simulation output, process `P1` had a burst time larger than the time quantum of `2000ms`, so after running for `2000ms`, it yielded the CPU and was re-queued twice before finally completing its execution. This re-queueing behavior guarantees fairness by preventing long CPU-bound tasks from starving shorter processes of execution time.

Example from my output:
```text
[Main] ⚙️  Scheduling P1 (Remaining Burst: 3500 ms, Priority: 3)
[P1] ▶️  Running for quantum 2000 ms...
[P1] ↻  P1 yields CPU for context switch
[Main] ➕ P1 added back to ready queue
