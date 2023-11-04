## Paper summary

In this paper, the authors describe the design considerations and architecture of Snowflake, a multi-tenant transactional system. The authors identify the problem to be that of a pure shared-nothing design becoming unsuitable for cloud environments where the software cannot scale and the workloads are unpredictable. The design consideration for Snowflake lies in the separation of storage and compute to overcome these drawbacks. This choice is made to accommodate heterogeneous workloads, manage node failures better and allow online upgrades with no downtime. The design of the system is as follows - both storage and compute are scalable services which communicate through RESTful interfaces, and the three architectural layers are data storage, virtual warehouses and cloud services.

### Data Storage

Amazon S3 is used for data storage and it follows that Snowflake’s table format is columnar and horizontally partitioned. Temp data from large queries is also stored in S3 to avoid out-of-memory errors. Write operations on a table can only be made by modifying entire files.

### Virtual Warehouse

Virtual warehouses are clusters of EC2 instances running worker nodes. For every new query, a new worker process is spawned. Only file headers and individual columns are cached, with a LRU cache replacement policy that is lazy. While file stealing is used to enable communication between peers, in case of straggler nodes, peers may transfer ownership.

### Cloud Services

The cloud services layer is multi-tenant. The query optimizer functions in a top-down manner, with many decisions postponed until execution. Concurrent reads are handled with Snapshot Isolation, which means that every read sees a consistent snapshot of the table. Unlike traditional data warehouses, a min-max pruning approach is used to reduce the metadata size for fast access, and dynamic pruning is also done.

According to the paper, there are four differentiators of the system - pure SaaS experience enhanced by the use of a web UI to interact with the system, resilience due to the use of S3 and online upgrades with no downtime, allowing accessibility to semi-structured and schema-less data by introducing data types such as VARIANT and columnar processing, and avoiding type conflict by storing the input and output of a type conversion independently. Additionally, Snowflake allows time travel and cloning, and has an extensive encryption process abstracted from the user for all-rounded security of the system. Experimentation is done by using standard data and queries against a relational system as well as Snowflake - on a high level, the results show that the two perform comparably.

## Comments and Questions
1. Why not compare the performance of Snowflake against the other systems mentioned in the paper, such as BigQuery, Presto, and Azure? The paper states that certain use cases cannot be served by these platforms but does not mention them in any detail.
2. What are the edge cases for which the system will not be unquestionably available during online upgrades? The paper states that there are no partial retries for large queries, could that be one of the cases?
3. How does the system actually scale in case of a user request?
4. Deeper analysis of many of the design considerations is skipped - use of S3 for data storage; use of LRU replacement policy; Is the min-max based pruning approach, while fast, also more effective?
5. Can view materialization for large queries help improve the performance of the system?
