How would you optimise a SQL join between two massive tables to avoid performance bottlenecks in Databricks?
	Initial intuition: Create hash indexing for the tables. 



==Explain the difference between bronze, silver and gold tables in the Madeline architecture, in the context of Delta Lakes. Why does this architecture matter?==


How does Sparks lazy execution model work and why is it important?
	- DAG creation using 'action' and 'transformation' tasks. Upon 'action' tasks which require shuffling need to create a new 'stage'. Essentially, stages are composed to multiple narrowly dependable 'tasks'. Df a task has a wide dependency then a new stage must be setup.
	- Helps in debugging and selective comands can be used to walk the stage tree backwards if there are issues in the executers. 


Describe how you would schedule and monitor a multiple-step data pipeline in databricks. What would you do when a step fails?

Give me an example of a data pipeline that you built that directly impacted the businesses KPI. How do you measure that success?


Talk about cluster sizing, spot instances, partition pruning. 


Explain the difference between all purpose and job clusters in databricks. When would you use each and optimise for cost?


