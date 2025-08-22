
While functional requirements define what the system does, **non-functional requirements** define how well it does it. For example, a system that can search videos but fails under high traffic won't be useful at scale.

The most important topics to focus on are shown below:

- **Scalability**: How many users does the system need to support, and will the system handle a large number of concurrent users or requests per second?
- **[CAP Theorem](https://systemdesignschool.io/domain-knowledge/distributed-database-cap-and-pacelc-theorem)**: Does your system need to prioritize availability or consistency? Remember, different parts of the system may have different needs (e.g., video serving might prioritize availability, but banking might prioritize consistency)
- **Read/Write Ratio**: Is the system read-heavy (e.g., many people searching and watching videos) or write-heavy (e.g., many content creators uploading new videos)? Understanding this can guide your choice of databases or storage solutions.
- **Latency**: How quickly does the system need to respond to user requests?

Here are some more topics you may want to address, depending on the question:

- **Security**: How secure should the system be? Consider factors like authentication, encryption, and access controls.
- **Durability**: How durable should the system be? What is the timeframe for data retention?
- **Fault Tolerance**: How fault-tolerant should the system be? How will it recover from failures and handle potential issues?

These should be worded in the format of "The system should be...", such as "The system should be able to support 100 million daily active users."

Non-functional requirements can often expose the **trade-offs** that define your design choices. For example:
- if you optimize for low-latency reads, how does that impact your write operations? 
- If the system is highly available, how will you handle potential data consistency issues?

The goal here is to demonstrate that your design can **scale, perform reliably, and handle real-world demands**. Handling these non-functional requirements well sets apart a good system design from a great one.