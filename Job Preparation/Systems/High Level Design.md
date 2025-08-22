Now that you have a clear understanding of the functional and non-functional requirements, as well as the primary API endpoints, you can start on the **high-level design**, which will be the key focus of the interview.

There are two parts to the high-level design: diagram, and explanation.

The diagram is done by dragging and dropping system components from our component library and connecting them with arrows in our interactive canvas below. Components are basic building blocks, such as:

- **Services**: Services contain the **business logic** of the system. You should ==have a service for every distinct part of your system.==
- **Database:** Your database is what stores the data. Don't just mention that you need a database; you should be ==explaining what kind of database==, why you chose that type, and give a general idea of what the schema might look like.
- **Cache:** A cache may be used to ==store frequently accessed data in a system to reduce queries to the database.==
- **Message Queue:** A message queue is used to decouple services and protects the entire ==system from going down in the event of a service failure.==

The explanation is what you put in the text box, and where you will explain how the varying parts of the system interact with one another. What separates a ==no-hire from a strong-hire is your ability to justify why you're making certain decisions==. There is no one correct answer, you just need to be able to explain why you're adding what you're adding, and being aware of trade-offs.

A good structure to follow for answering this is to do each functional requirement at a time, building off of your previous diagram for each answer.