In the previous part of this series (OnLinkedIN (https://www.linkedin.com/in/abhijithvamadev/) ), we introduced Apache Spark and its execution model. In this section, we take a deeper look at RDDs (Resilient Distributed Datasets), the fundamental data abstraction in Spark.


# What is an RDD?
An RDD is a distributed, immutable collection of objects that is processed in parallel across a Spark cluster.
- Data loaded into Spark is stored inside Spark objects.
- Data loaded into a Spark object, is represented as an RDD
- RDDs are typically stored in memory, but can spill to disk if needed

## Partitions and Data Locality
Partitioning is fundamental to Spark’s performance model.
Data is split into multiple partitions
Each partition contains a subset of the overall dataset
Partitions are distributed across worker nodes
Example:
Block 1 → records 1, 2
Block 2 → records 3, 4
Block 3 → record 5

Data is loaded into the RAM of the worker node that already contains the data block. This minimizes network transfer and improves execution speed

## Transformations and Immutability
RDDs are immutable, meaning their contents cannot be changed once created. Any transformation produces a new RDD, Original RDDs remain unchanged.
This enables fault tolerance. Transformations are user-defined operations.
Spark Job Execution Workflow
1. The programmer launches a client application
2. A SparkContext is created and lives for the duration of the job
3. The Cluster Manager (YARN or Mesos) allocates resources
4. Jobs are broken into smaller tasks
5. Tasks execute on individual worker nodes
Results are either:
1. Returned to the driver (only for small datasets)
2. Written back to HDFS or another distributed storage system
Returning large datasets to the driver is not recommended, as the data must fit within the JVM heap memory.

## Lazy Evaluation and Actions
Spark uses lazy evaluation.
Transformations build a Directed Acyclic Graph (DAG)
Execution does not begin until an action is called.
Action is invoked, Spark executes the entire DAG from start to finish.

## Fault Tolerance and Lineage
RDDs are resilient, meaning they are fault-tolerant.
Spark tracks Lineage. Lineage records how an RDD was derived from previous RDDs
If an RDD partition is lost, Spark re-executes only the required transformations. Allows Spark to recover efficiently, even with thousands of RDDs.

## Caching RDDs
1. Spark allows RDDs to be cached in memory.
2. Cached RDDs are stored in the memory of executors
4. Caching is explicitly controlled by the developer
3. Useful when the same dataset is reused multiple times

## Ways to Create RDDs
1. RDDs can be created in three primary ways:
2. Reading data from an external file system (HDFS, S3, local)
3. Using the parallelize API (typically for small datasets)
4. Using the makeRDD API

Additional Resources
For more details on RDDs, transformations, and actions:
https://spark.apache.org/docs/latest/rdd-programming-guide.html 
