# Paper Summary

This paper discusses the evolution of large-scale data analysis trends originated from Dremel.

## SQL
There has been a trend of migrating away from SQL and then back: Dremel re-introduced SQL at Google. Dremel’s SQL dialect was easy-to-use, supported structured data and allowed querying nested data. Several of Google’s systems used a different SQL dialect, motivating the development of a common unifying framework, GoogleSQL (now the open-source ZetaSQL). However, portability of SQL dialects across engines remains a challenge.

## Disaggregated Storage
The second trend was of disaggregated storage. Initially, Dremel used dedicated disk storage for faster query analysis. As the workloads grew, Dremel was moved to a cluster management system and eventually, to a replicated storage. Replicated storage was disadvantageous as the storage and processing were tightly coupled. Finally, Dremel was migrated to Google’s File System (GFS) to have shared storage rather than local disks. Dremel provided support for distributed joins using a shuffle infrastructure that stored intermediate results to local RAM. This was a scalability bottleneck, and therefore prompted the development of a separate transient storage for intermediate shuffle data - memory disaggregation. The key observation is that storage disaggregation has been universally adopted by OLAP and OLTP systems.

## In-situ data analysis
The third trend of in-situ data processing moved away from explicit data loading before querying in part due to a self-describing columnar data format. Eventually, Google added support for more file formats such as CSV and JSON and expanded to data federations to use the strengths of remote file systems. The main disadvantage of in-situ analysis was the assumption that users could manage their own data. Eventually, a hybrid approach supporting both managed and in-situ data was developed.

## Serverless computing
The fourth trend of serverless computing was pioneered by Dremel, moving away from single-tenant resource provisioning (MapReduce and Hadoop). The deterministic and repeatable nature of the subtasks of a query accounted for unresponsive compute resources and imparted fault tolerance. The initial ideas were further developed to give rise to a centralized scheduling which uses the state of the entire cluster for making decisions and a shuffle persistence layer that makes decoupling of scheduling and execution of different stages of the query easier. Finally, query execution was modelled as a dynamic Directed Acyclic Graph rather than a static tree.

## Columnar storage format for nested data
The fifth trend was that of columnar storage for nested data which was later adopted by Twitter for the parquet format and also for ORC and Apache Arrow. Dremel proposed the use of repetition and definition, while ORC tracked the lengths of repeated fields and a boolean attribute to indicate the presence of optional fields. While the repetition/definition format encoded all information about the structure as compared to length/presence, it also led to redundancy as observed by a big difference in the encoding file sizes for highly-nested datasets. Eventually, an improved columnar format, Capacitor, was developed. It maintains statistics about all the columns to simplify filtering, enables skipping of segments to make for fast filtering, uses run-length encoding for optimizing sorting and allows more complex schemas.

Finally, the paper also talks about other contributions of Dremel including strategies for capacity-reservation and latency reduction. It concludes by re-iterating the instant and eventual hits resulting from the paper on Dremel's architecture and the misses which served as starting points for modified system designs.

# Comments and Questions
The paper addresses key trends in a clear and precise manner. At some points, the information is hard to evaluate since the paper is structured to provide a glimpse of changes across a large duration of time. However, the paper does a good job of briefly describing many of the important changes inspired by Dremel, the motivations and the industry-wide implications. Infusing example-based descriptions of certain changes such as those of the shuffle persistence layer and run-length encoding for row ordering make the paper accessible to readers without prerequisite knowledge of Dremel. The paper also lends insight into active areas of research such as row ordering.

Q. I would be interested to know how data federations work, and what are some of the most significant advantages of the common federations
Q. The paper sheds some light on the evolution of the scheduling system within Dremel, and I would like to know more about it in the context of Dremel as well as other query engines
Q. Adding onto the key contributions of Dremel, the paper also describes other latency-reducing techniques such as the “approximate results” strategy - I would be interested to know if other systems have also adopted something similar to this
