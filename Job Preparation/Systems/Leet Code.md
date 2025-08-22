To design the code evaluation flow for our LeetCode-like platform, we need to consider the **high concurrency requirement** of **10,000 users participating in contests simultaneously**, each submitting solutions about **20 times on average**. This is a classic use case using our [design template](https://systemdesignschool.io/fundamentals/system-design-template).

We use a **Code Evaluation Service** as the entry point for code submissions. This service will be responsible for receiving code from users and initiating the evaluation process. However, to handle the high volume of submissions efficiently and securely, we introduce a **Message Queue** as a buffer between the submission process and the actual code execution.

Note that most system design questions requires microservices. For a simpler problem like this, we can use a single service to handle both problem service and code evaluation. However, code submission can be compute heavy and we do not want to bring down the entire service when handling a large number of submissions. Therefore, we opt to separate the two services.

![Leetcode Design Diagram 1 Submit Solution](https://systemdesignschool.io/solutions/leetcode/leetcode_design_diagram_1_submit_solution.png)![](https://systemdesignschool.io/search.svg)

When a user submits a solution, the Code Evaluation Service pushes a message containing the submission details (user ID, problem ID, and code) to the Message Queue. This **decouples the submission process from the execution process**, allowing us to handle **traffic spikes** more effectively.

On the other side of the Message Queue, we have multiple **Code Execution Workers**. These workers continuously poll the queue for new submissions. When they receive a message, they fetch the necessary test cases from the Problems database, **execute the code in a sandboxed environment**, and record the results. Let's compare different execution options for the sandbox environment:

{{ comparisons.code_execution_options }}

We will choose **Container (Docker)** over serverless functions for the faster startup time and less vendor lock-in.

To ensure **security and isolation**, each submission runs in a **separate container**, preventing malicious code from affecting our system or other users' submissions. We can use technologies like **Docker** for this purpose.

After execution, the results (including whether the code passed all test cases, execution time, and memory usage) are saved in the Submissions table.

#### Async Result Retrieval

Since code execution can take time (especially for complex problems or when the system is under heavy load), the submission process is **asynchronous**. Here's how the frontend handles getting results:

1. **Submission Response**: When a user submits their solution, the Code Evaluation Service immediately returns a **submission ID** rather than waiting for execution to complete.
    
2. **Polling Mechanism**: The client uses this submission ID to **poll the backend periodically** (typically every 1-2 seconds) via an endpoint like `GET /submissions/{submission_id}/status`.
    
3. **Status Updates**: During polling:
    
    - If the submission is still being processed, the endpoint returns a status like `"pending"` or `"processing"`
    - Once execution completes, the endpoint responds with the final result, including:
        - Overall status (`success`, `fail`, `timeout`)
        - Individual test case outcomes
        - Execution details (time, memory usage)

This is actually how LeetCode handles the result retrieval. We can see it in the Chrome DevTools:![LeetCode polling endpoint](https://systemdesignschool.io/solutions/leetcode/leetcode-polling-endpoint.png)![](https://systemdesignschool.io/search.svg)

What about WebSocket or Server-Sent Events?

- WebSocket is overkill for simple status updates. It's designed for bidirectional, real-time communication, but we only need one-way status updates at predictable intervals.
    
- SSE still requires keeping long-lived HTTP connections open, which can be resource-intensive when managing thousands of concurrent users during contests.
    

Since the polling load is predictable (after submission, frontend polls every 1-2 seconds only while waiting for results), the additional complexity of real-time connections isn't justified. Simple HTTP polling provides adequate user experience while being easier to implement.


## Leader Board Design:

We want to use a in-memory database like Redis because it's updated very frequently and older data can be discarded. **Redis actually comes with a built-in leaderboard implementation.** The data structure behind it is a **sorted set (ZSET)**. Sorted sets combine unique elements (members) with a score, and they keep the elements ordered by the score automatically.

#### Sorted Set Internals
Internally, the sorted set is implemented as a hash map and a [skip list](https://en.wikipedia.org/wiki/Skip_list). The hash map stores the mapping between the user IDs and the scores. The skip list keeps the scores in ascending order. Skip list is not a common data structure, so it's worth explaining it here.

- **Skip list**: A skip list is a **probabilistic data structure** that allows for fast search, insert, and delete operations. It's a series of linked lists with additional pointers to intermediate nodes, allowing for efficient traversal and search operations. **Functionally, it's similar to a balanced binary search tree, but it's implemented using linked lists and randomization**. The time complexity of search, insert, and delete operations is **O(log n)** on average. The randomization is used to ensure that the skip list is balanced, so the performance is good even if the list is not balanced. This is similar to how binary search trees need to be balanced by rotating nodes. Finally, the skip list can return top N in **O(N + log M) time** where N is the number of elements to return and M is the number of elements in the list.

Here's an example of a skip list:

```
Level 3:  [ 1 ]-------------------------------------> [ 20 ]
            |                                           |
Level 2:  [ 1 ]-----------------> [ 7 ]-------------> [ 20 ]
            |                       |                   |
Level 1:  [ 1 ]-----> [ 3 ]-----> [ 7 ]------> [ 15 ]-[ 20 ]
            |           |           |             |     |
Level 0:  [ 1 ] [ 2 ] [ 3 ] [ 5 ] [ 7 ] [ 10 ] [ 15 ] [ 20 ] [ 25 ]
```

- **Levels**: Higher levels have fewer elements, allowing "skips" to speed up searching. For example, level 3 can directly skip from node 1 to 20, avoiding unnecessary steps.
- **Nodes**: Each node holds a value (e.g., 1, 3, 7, etc.). At level 0, every node is linked like a standard linked list.
- **Links**: Vertical links connect nodes across different levels, while horizontal links connect nodes at the same level.

The search works by starting at the highest level and moving down as needed:

- Start at the highest level (e.g., Level 3).
- Move right until the next node’s value is greater than or equal to the target.
- Move down a level if the next value is too large.
- Repeat this process at each level, moving right as far as possible, then down, until the target is found or confirmed absent.

Example: Searching for 15

- Start at node 1 on Level 3, drop down since 20 > 15.
- On Level 2, move from 1 to 7, then drop down again.
- On Level 1, move to 15 and stop.

This approach gives an average O(log n) time complexity by efficiently skipping unnecessary nodes.

The skip list is a series of linked lists with additional pointers to intermediate nodes, allowing for efficient traversal and search operations.

To get the leaderboard, i.e. get the top `N` users, we can go down to the level 0 and find the last element in the list, and then perform a backward iteration to get the top `N` users (the linked lists in a skip list are doubly linked).

In Redis, this is implemented as `ZSET`. We use `ZADD` to add a user and its score, and use `ZRANGE` to get the top `N` users.

#### Leaderboard Example

Here's a simple example:

`ZADD leaderboard 100 user1 ZADD leaderboard 200 user2 ZADD leaderboard 300 user3 ZREVRANGE leaderboard 0 1 # returns the top 2 users`

## Ensure Security and Isolation in code execution:

Running the code in a sandboxed environment like a container already provides a measure of security and isolation. We can further enhance security by limiting the resources that each code execution can use, such as CPU, memory, and disk I/O. We can also use technologies like `seccomp` to further limit the system calls that the code can make.

To be specific, to prevent malicious code from affecting other users' submissions, we can use the following techniques:

- **Resource Limitation**: Limit the resources that each code execution can use, such as CPU, memory, and disk I/O.
- **Time Limitation**: Limit the execution time of each code execution.
- **System Call Limitation**: Use technologies like `seccomp` to limit the system calls that the code can make.
- **User Isolation**: Run each user's code in a separate container.
- **Network Isolation**: Limit the network access of each code execution.
- **File System Isolation**: Limit the file system access of each code execution.

Here's sample command while running `docker run ...`:

`--cpus="0.5" --memory="512M" --cap-drop="ALL" --security-opt="seccomp=unconfined" --network="none" --read-only`

This limits the code execution to use at most 0.5 CPU, 512 MB of memory, and disable all capabilities


## Handling Sudden Spikes:

Handling a large number of contestants submitting solutions simultaneously presents several challenges, particularly during peak times like the beginning and end of a contest. We can address this using a [two-phase processing](https://systemdesignschool.io/fundamentals/two-stage-processing) approach combined with other strategies:

1. **Message Queue**: Use a message queue to decouple the submission process from the execution process. This allows us to buffer submissions during peak times and process them as resources become available.
    
2. **Pre-scaling**: Since auto-scaling may not be fast enough to handle sudden spikes, pre-scale the execution servers before the contest starts. This is especially important for the first and last 10 minutes of a contest when submission rates are typically highest.
    
3. **Two-Phase Processing**:
    
    - **Phase 1 (During Contest)**: Implement partial evaluation
        - Run submissions against only about 10% of the test cases.
        - Provide a partial standing for users, giving them immediate feedback.
    - **Phase 2 (Post-Contest)**: Complete full evaluation
        - After the contest deadline, run the submissions on the remaining test cases.
        - Determine the official results based on full evaluation.

This two-phase approach:

- Reduces the load on the system during the contest.
- Allows for more accurate final results.
- Is similar to the approach used by platforms like Codeforces for large contests.

This approach immediate feedback for users with efficient resource utilization. While it means that final results won't be available immediately after the contest ends, it allows for handling a much larger number of concurrent submissions with existing resources. The trade-off between real-time complete results and system scalability is often necessary for large-scale coding competitions.
