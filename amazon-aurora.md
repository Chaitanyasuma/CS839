### Paper Summary 

This paper describes the design considerations for Amazon’s Aurora, a high-throughput cloud-native relational database. The authors clearly define the problem - the bottleneck in distributed systems has evolved from storage and compute to the network. The authors describe the traditional techniques of synchronous operations and transaction commits to be unsuitable for high-scale distributed systems. Therefore, they propose a service-oriented architecture which decouples several functions such as redo logging from the database to the storage.

There are three main contributions of this work - durability, smart storage, and consistency. Aurora achieves durability through a robust quorum-based replication strategy and segmented storage, also ensuring resilience to shorter outages. Aurora achieves smart storage by offloading log processing to a storage service, with only redo log and metadata writes crossing the network. Aurora performs significantly better than mirrored SQL, demonstrating the advantages of parallel processing and high availability. Finally, Aurora achieves consistency by maintaining state in an asynchronous manner that is inexpensive. Crash recovery is enabled in the background by offloading the redo log applicator to storage.

Aurora’s performance improvements are supported by data facts generated for at least a year before publishing of the paper (2015-2017). Standard benchmarks of instance sizes, data sizes and user connections indicate Aurora’s performance gains over MySQL. The authors also share examples of improved results in response time, latency and replica lag from the adoption of Aurora by various companies.

### Comments and Questions

The authors refer to the most recent work and describe commonly used approaches before delving into their contributions. This helps to understand the state-of-the-art, its disadvantages and the novelty of Aurora. Overall, the paper is organized in a way that effectively communicates the strengths of Aurora over traditional relational databases.

The paper describes Aurora's cache reading strategy but provides no experimental results for the cache misses. The results can be added to understand how caching can be optimized further. Although Aurora is designed for OLTP applications, it would be interesting to observe its performance for more complex queries.
