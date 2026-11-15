# OS Laboratory Activity 3
## Multilevel Queue and Real-Time CPU Scheduling

### Student Information
- Name: Marie June G. Lomo
- Course and Section: BCA151 - B186
- Instructor: Ma'am Maria Fe Bahinting
- Date Submitted: November 16, 2026

### I. Introduction
This laboratory activity implements and simulates Multilevel Queue (MLQ) scheduling and two real-time scheduling algorithms, Rate Monotonic Scheduling (RMS) and Earliest Deadline First (EDF), using Python. The Gantt charts, CPU utilization, waiting time, turnaround time, and deadline compliance of each algorithm are analyzed, and GitHub is used to manage and document the work.

### II. Objectives
- Explain the operation of Multilevel Queue, RMS, and EDF scheduling algorithms.
- Implement MLQ scheduling using Round Robin and FCFS.
- Implement preemptive RMS and EDF scheduling simulations using Python.
- Generate and interpret Gantt charts showing process and task execution.
- Calculate CPU utilization, waiting time, turnaround time, and deadline compliance.
- Create and manage a GitHub repository containing Python source code and laboratory documentation.

### III. Scheduling Algorithms
1. **Multilevel Queue Scheduling** - Processes are divided into separate queues, and each queue has its own scheduling algorithm. Queue 1 (interactive) uses Round Robin with a 2 ms quantum, and Queue 2 (batch) uses FCFS. Queue 1 has strict priority over Queue 2.
2. **Rate Monotonic Scheduling** - A fixed-priority preemptive algorithm for periodic tasks. The task with the shortest period gets the highest priority.
3. **Earliest Deadline First** - A dynamic-priority preemptive algorithm. The ready task with the earliest absolute deadline runs first.

### IV. Program Implementation
- **mlq.py** (`multilevel_queue/`): Simulates the CPU in 1 ms steps. Arriving processes are placed in Queue 1 or Queue 2. If Queue 1 has a process, it runs using Round Robin with a quantum of 2 ms. Queue 2 runs (FCFS) only when Queue 1 is empty, so a new Queue 1 arrival preempts the running Queue 2 process, which resumes later with its remaining burst time. The program prints the Gantt chart, completion, turnaround, and waiting times.
- **rms.py** (`real_time_scheduling/`): Creates a separate job for every release of each periodic task, then simulates the CPU in 1 ms steps for 20 ms. Among the ready jobs it always runs the one whose task has the shortest period (fixed priority). It prints the CPU utilization, the RMS utilization bound, the Gantt chart, and the deadline status of every job.
- **edf.py** (`real_time_scheduling/`): Uses the same job generation and 1 ms simulation as RMS, but selects the ready job with the earliest absolute deadline (release time + relative deadline), with release time and task ID as tie-breakers.

### V. Test Data
**MLQ (Round Robin quantum = 2 ms for Queue 1, FCFS for Queue 2)**

| Process | Arrival Time (ms) | Burst Time (ms) | Queue |
| --- | --- | --- | --- |
| P1 | 0 | 5 | 1 |
| P2 | 1 | 4 | 1 |
| P3 | 0 | 6 | 2 |
| P4 | 2 | 3 | 2 |
| P5 | 3 | 2 | 1 |

**RMS and EDF (simulation time = 20 ms)**

| Task | Execution Time (C) | Period (T) | Relative Deadline (D) |
| --- | --- | --- | --- |
| T1 | 1 ms | 4 ms | 4 ms |
| T2 | 1 ms | 5 ms | 5 ms |
| T3 | 2 ms | 10 ms | 10 ms |

### VI. Results and Discussion

#### A. Multilevel Queue
Gantt chart:

> | P1 [0-2] | P2 [2-4] | P1 [4-6] | P5 [6-8] | P2 [8-10] | P1 [10-11] | P3 [11-17] | P4 [17-20] |

| Process | AT | BT | CT | TAT | WT |
| --- | --- | --- | --- | --- | --- |
| P1 | 0 | 5 | 11 | 11 | 6 |
| P2 | 1 | 4 | 10 | 9 | 5 |
| P3 | 0 | 6 | 17 | 17 | 11 |
| P4 | 2 | 3 | 20 | 18 | 15 |
| P5 | 3 | 2 | 8 | 5 | 3 |

Average Turnaround Time: 12.0 ms. Average Waiting Time: 8.0 ms.

All Queue 1 processes (P1, P2, P5) finished before any Queue 2 process (P3, P4) got the CPU, even though P3 arrived at time 0. This confirms that Queue 1 has strict priority over Queue 2. The Queue 2 processes had the longest waiting times as a result.

#### B. Rate Monotonic Scheduling
Priority order: T1 > T2 > T3. CPU utilization: 65%. RMS sufficient bound for 3 tasks: 77.98%. Sufficient-bound test: PASS.

Gantt chart:

> | T1 [0-1] | T2 [1-2] | T3 [2-4] | T1 [4-5] | T2 [5-6] | IDLE [6-8] | T1 [8-9] | IDLE [9-10] | T2 [10-11] | T3 [11-12] | T1 [12-13] | T3 [13-14] | IDLE [14-15] | T2 [15-16] | T1 [16-17] | IDLE [17-20] |

| Task Instance | Release | Deadline | Completion | Status |
| --- | --- | --- | --- | --- |
| T1 | 0 | 4 | 1 | MET |
| T2 | 0 | 5 | 2 | MET |
| T3 | 0 | 10 | 4 | MET |
| T1 | 4 | 8 | 5 | MET |
| T2 | 5 | 10 | 6 | MET |
| T1 | 8 | 12 | 9 | MET |
| T2 | 10 | 15 | 11 | MET |
| T3 | 10 | 20 | 14 | MET |
| T1 | 12 | 16 | 13 | MET |
| T2 | 15 | 20 | 16 | MET |
| T1 |
