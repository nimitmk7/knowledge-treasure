## Premise
The growth of data volumes in industry and research poses tremendous opportunities, as well as tremendous computational challenges. 

As data sizes have outpaced the capabilities of single machines, users have needed new systems to scale out computations to multiple nodes. As a result, there has been an explosion of new cluster programming models targeting diverse computing workloads.

But most big data applications need to combine many different processing types. The very nature of “big data” is that it is diverse and messy; a typical pipeline will need MapReduce-like code for data loading, SQL-like queries, and iterative machine learning. 

Specialized engines can thus create both complexity and inefficiency; users must stitch together disparate systems, and some applications simply cannot be expressed efficiently in any engine.

The intent behind Apache Spark is to design a unified engine for distributed data processing. It has a programming model similar to [[MapReduce]] but extends it with a data-sharing abstraction called “Resilient Distributed Datasets” or RDDs. 

Using this simple extension, Spark can capture a wide range of processing workloads that previously needed separate engines, including **SQL**, streaming, **machine learning**, and **graph processing**. 

These implementations use the same optimizations as specialized engines (such as column-oriented processing and incremental updates) and achieve similar performance but run as libraries over a common engine, making them easy and efficient to compose.

![Apache Spark Software Stack, with specialized processing libraries implemented over the core engine.](spark_stack.png)
## Benefits

1. **Easier application development** because they use a unified API.
2. **More efficient to combine processing tasks** 
	1. whereas prior systems required writing the data to storage to pass it to another engine, Spark can run diverse functions over the same data, often **in memory**.
3. **Enables new applications** that were not possible with previous systems.
	1. Like Interactive graph queries, Streaming Machine Learning

## Programming Model : [[Resilient Distributed Datasets - RDDs]]

## Applications

1. Batch Processing
2. Interactive queries
3. Streaming Processing
4. Scientific Applications

## References
1. [Apache Spark: A Unified Data Engine for Big Data Processing](https://people.eecs.berkeley.edu/~alig/papers/spark-cacm.pdf) 