
## ETL : Extract Transform Load

The preparation processes that data goes through for storage:
1. `Extract`: 
	1. Take data that is in raw/semi-structured format and convert to structured
	2. Data may come from multiple different sources
2. `Transform`: 
	1. Convert units, join data sources, cleanup
	2. Transform data into a consistant format
3. `Load` : 
	1. Loading it into another system (central data warehouse or data lake) for further processing.


## Programming Languages:
- Java is considered the 'assembly' of most big data infrastructure. 
- ==Scala is compiled== using the JVM ByteCode. 
- Scala’s strong point is the combination of Functional Programming and Object Oriented

1. Declarations:
	1. _Type inference_ used extensively
	2. Two types of variables: `val`s are single-assignment, `var`s are multiple assignment
	3. `val` : read only
	4. `var` : read and write
2. Declaring Functions:
	1. Statically typed, while also being type inferred. 
	2. Evaluated _expressions_ have types
	3. The return type is the most generic type of all return expressions
3. Higher Order Functions:
	- Higher-order function is a function whose behaviour is parametrised by another function.
	- Need to pass a function with the appropriate argument types. The compiler checks this in the case of Scala
4. Data Classes:
	- Data classes are blueprints for immutable objects. We use them to represent data records. Both languages implement `equals` (or `__eq__`) for them, so we can compare objects directly.