
TODO, Summarise this: https://equalocean.com/news/201903031507

Key Words: Failure isolation, Redundant replication, thread pool exhaustion

---
Interview Questions:
1. For partial failures what are some techniques that can be used to detect partial failures?
2. What are some design choices, and tools are used to ensure partial failures are avoided?
3. Which clock type would you use to benchmark a routine? Why? 
	1. System.getCurrentTimeMillis
	2. Monotonic Clocks
4. Difference between physical time and logical time.
	1. Physical = wall-clock (e.g., NTP-synced); logical = Lamport/Vector timestamps.


---

extensions:
- What is Cristian’s algorithm?
- What is Berkeley’s algorithm?
- Explain Google’s TrueTime API

---

- 4 types of failures in distributed systems:
- A partial failure of one elements should not render the whole system unable to functions as a whole. 
- It is also hard to detected what has failed as it takes time for a message to travel across a network. 

## Partial Failures : Case study Alibaba Cloud incident

- Not all components fail, and the system may keep running. However inconsistent behaviours may be detected.

- **Systems Engineering Lessons**

|Challenge|Why it matters in partial failure|Case link|
|---|---|---|
|**Failure Isolation**|Limit blast radius so one zone or rack failure doesn’t cascade|Only _Region C_ hit — shows some isolation was in place|
|**Graceful Degradation**|System should continue to serve partial workloads|If failover to other zones was faster, downtime could be shorter|
|**Observability**|Need fine-grained metrics to detect subtle issues|I/O hangs may require per-device latency alarms|
|**Automated Failover**|Manual intervention = slower recovery|Incident needed “urgent maintenance” → possible human-in-the-loop bottleneck|
|**Capacity Headroom**|Other zones must absorb sudden rerouting|Distributed load balancing needs surge capacity|

## Network Systems:
- Networks are unreliable, and the goal is to synchronous communication
- The goal requires you to be able to:
	- Have timed failure detections <- not possible because ..?
	- Time based communication <-- not possible because ..?
	- Worst case performance <-- not possible because ..?
- The goal for a network is to make an inherently asynchronous communication network -> synchronous.

## Unreliable Time

-  Network Time Protocol (NTP) is ==a networking protocol used to synchronize the clocks of computer systems over a network==.


### Lamport Time vs Vector Clock:
|Feature|Lamport Timestamp|Vector Clock|
|---|---|---|
|**Space per process**|O(1)|O(N)|
|**Causality detection**|No (only total order)|Yes (happened-before + concurrency)|
|**Overhead**|Low|Higher (vector grows with system size)|
|**Ordering guarantee**|If A→B then L(A)<L(B)|If A→B then V(A)<V(B)|
|**Concurrency detection**|No|Yes|
|**Implementation**|Simple|More complex

# Decision Making:
