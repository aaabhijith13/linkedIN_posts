# Big Data Series (Part 2/x): Deep Dive into HDFS

In Part 1 of this series, we introduced Hadoop and its core components. This section provides a deeper look into HDFS (Hadoop Distributed File System), the storage layer responsible for reliable and scalable data persistence in Hadoop.

HDFS is designed to store very large datasets across a cluster of machines. Instead of relying on a single disk or server, HDFS distributes data across multiple nodes, allowing the system to scale horizontally and tolerate failures.

## File Block Architecture
- When a file is written to HDFS, it is automatically divided into fixed-size blocks.
- Default block size: 64 MB (configurable)
- Blocks are distributed across different DataNodes
- Example:
- Input file size: 200 MB
- Block breakdown:
- Block 1: 64 MB
- Block 2: 64 MB
- Block 3: 64 MB
- Block 4: 8 MB
This block-level distribution enables parallel data access and processing.

### Role of the NameNode
The NameNode is the central authority for HDFS metadata management.
It maintains:
- File-to-block mappings
- Block-to-DataNode mappings
- File permissions and configuration
- Replication factor information
The NameNode does not store actual data. Instead, it acts as a metadata service that allows clients to locate data efficiently.

### DataNodes and Heartbeats
DataNodes are responsible for storing actual data blocks.
Their responsibilities include:
- Reading and writing data blocks
- Replicating blocks as instructed by the NameNode
- Sending periodic heartbeat signals to indicate health and availability
If a DataNode stops sending heartbeats, the NameNode marks it as unavailable and triggers re-replication.

### Data Replication and Fault Tolerance
To handle node failures, HDFS replicates each data block across multiple nodes.
1. Replication factor: Number of copies maintained per block
Ensures data availability even if one or more DataNodes fail
This replication mechanism is the foundation of Hadoop’s fault tolerance model.
2. Replica Placement Strategy (Rack Awareness) - HDFS uses a rack-aware placement strategy to optimize reliability and performance.
The NameNode places replicas based on network topology
3. Typical default strategy:
i) First replica placed on the local node
ii) Second and third replicas placed on a different rack
This approach minimizes network traffic while protecting against rack-level failures. 
iii) Clients always attempt to read data from the closest available replica.

### HDFS Access Pattern Characteristics
HDFS is optimized for specific workloads:
Well-suited for:
1. Very large files (hundreds of MBs to petabytes)
2. Streaming and batch reads
3. Write-once, read-many (WORM) access patterns
Not well-suited for:
1. Large numbers of small files
2. Low-latency random reads
3. Frequent writes or in-place file updates
4. Once a file is written to HDFS, it cannot be modified; it can only be appended or rewritten entirely.

Data below shows Data Replication and working of HDFS architecture.

![HDFS architecture and NameNode](../images/post2)
