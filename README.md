# Assignment-4-of-unit-4-by-bhoomika-kalyan-
assignment 4 completed of mongodb subject 
Name: Bhoomika Kalyan
CU ID: CU24250015
Class & Section: BCA & A
Assignment: Assignment 4 – Unit 4
Subject: NoSQL Database
📄 PDF: Download Assignment 4 – Unit 4 PDF
ASSIGNMENT – UNIT 4: NoSQL DATABASE
Question 1
Explain the concept of Column-Oriented NoSQL Databases. How does the column-family model differ from the traditional relational table model? Discuss the advantages and limitations of using column-oriented databases for large-scale applications.
Answer:
A column-oriented NoSQL database stores data in column families rather than fixed relational tables. Examples include Apache HBase and Apache Cassandra. These databases are designed for very large, distributed datasets and high-volume read/write workloads.
In a column-family model, a row is identified by a row key and can contain a variable set of columns. Columns are grouped into column families. Different rows do not necessarily need to contain exactly the same columns.
For example:
Row Key: S101
profile:name = Bhoomika
profile:course = BCA
activity:last_login = 2026-10-06
activity:device = Android
In a relational database, a table normally has a predefined schema with fixed columns, data types, primary keys and foreign keys. Rows generally follow the same structure.
Advantages:
Horizontal scalability – Data can be distributed across many machines.
High write throughput – Suitable for logs, events and activity streams.
Flexible schema – Different rows can have different columns.
High availability – Replication provides fault tolerance.
Efficient access – Data can be modeled according to application queries.
Limitations:
Data modeling requires careful planning.
Joins and complex relational queries are not the main strength.
Poor row or partition keys can create hotspots.
Transactions are more limited than traditional RDBMS transactions.
Distributed cluster management can be complex.
Thus, column-family databases are useful when scalability, availability and high-volume processing are more important than complex joins and traditional relational querying.

Question 2
Compare Apache HBase and Apache Cassandra with respect to data model, architecture, consistency, availability, scalability, fault tolerance, query processing, and suitable applications. Which one would you choose for a highly distributed application and why?
Answer:
Apache HBase and Apache Cassandra are both distributed column-family NoSQL databases, but they have different architectures.
Feature
HBase
Cassandra
Data Model
Row key + column families
Partition key + clustering columns
Architecture
Master/RegionServer based
Peer-to-peer
Consistency
Generally strong row-level consistency
Tunable consistency
Availability
High
Very high
Scalability
Horizontal
Horizontal
Storage
HDFS
Distributed Cassandra storage
Query
APIs, Get, Put, Scan, Filters
CQL
Best suited for
Hadoop/HDFS-based systems
Highly distributed, always-on applications
HBase:
HBase works closely with the Hadoop ecosystem and HDFS. It is suitable for large datasets requiring random, near-real-time access.
Cassandra:
Cassandra uses a peer-to-peer architecture. It supports replication, tunable consistency and multi-datacenter deployment.
Choice:
For a highly distributed application, I would choose Apache Cassandra because it provides:
Peer-to-peer architecture
High availability
Multi-datacenter support
Tunable consistency
Easy horizontal scaling
Fault tolerance through replication
However, if an application requires strong integration with Hadoop/HDFS, HBase may be a better choice.

Question 3
Explain the architecture of HBase in detail. Describe the roles and interactions of HMaster, RegionServer, ZooKeeper, HDFS, Regions and MemStore during read and write operations. Draw a suitable architecture diagram.
Answer:
Apache HBase is a distributed column-family database that runs on top of HDFS.
Main components:
1. HMaster:
HMaster performs administrative operations such as table creation, deletion, region assignment and load balancing.
2. RegionServer:
RegionServers handle actual read and write operations. Each RegionServer contains one or more regions.
3. Region:
A region is a portion of an HBase table containing a range of row keys. Large regions can be split into smaller regions.
4. ZooKeeper:
ZooKeeper provides distributed coordination and helps maintain cluster information and service discovery.
5. HDFS:
HDFS provides persistent distributed storage for HBase data.
6. MemStore:
MemStore is an in-memory structure where recent writes are temporarily stored before being flushed to HFiles on HDFS.
Write operation:
Client identifies the appropriate RegionServer.
Data is sent to the RegionServer.
The operation is recorded in the Write-Ahead Log (WAL).
Data is placed in MemStore.
MemStore is eventually flushed to HFiles on HDFS.
Read operation:
Client locates the required region.
RegionServer checks MemStore and HFiles.
Relevant data is combined and filtered.
Result is returned to the client.
Architecture:
                 Client
                   |
                   v
              ZooKeeper
                   |
          +--------+--------+
          |                 |
       HMaster        RegionServers
                           |
                 +---------+---------+
                 |                   |
              Region 1            Region 2
              MemStore            MemStore
                 |                   |
                 +---------+---------+
                           |
                          HDFS
                       HFiles/WAL

Question 4
Explain the Column-Family Data Store model with the help of a suitable example. Discuss concepts such as row key, column family, column qualifier, cell, timestamp, and versioning. How is this model different from rows and columns in an RDBMS?
Answer:
A column-family data store represents data as rows identified by row keys, with columns grouped into column families.
Example:
Table: Student

Row Key: S101

profile:name       = Bhoomika
profile:course     = BCA
profile:semester   = 4
activity:last_login = 2026-10-06
Important concepts:
Row Key:
Unique identifier of a row. Example: S101.
Column Family:
Logical group of related columns. Example: profile and activity.
Column Qualifier:
Specific column within a family. Example: name in profile:name.
Cell:
The actual stored value identified by row key, family, qualifier and timestamp.
Timestamp:
The time associated with a stored cell value.
Versioning:
Multiple versions of a value can be stored depending on database configuration.
Difference from RDBMS:
RDBMS has fixed columns, while NoSQL column families can be sparse.
HBase can maintain multiple versions of cells.
RDBMS commonly uses joins and foreign keys.
Column-family databases generally avoid joins.
NoSQL systems are designed for horizontal scaling.

Question 5
A university needs to store billions of student activity records, where different students may have different attributes. Design a suitable HBase data model for this application and justify your choice of row key and column families.
Answer:
A suitable HBase table can be designed as:
Table: StudentActivity

Row Key:
<student_id>#<time_bucket>
For example:
S101#2026-10
For very high traffic, a salt/hash prefix can be added:
07#S101#2026-10
Column Families:
1. profile
name
course
semester
department
2. activity
type
device
page
login_time
3. academic
subject
attendance
result
Justification:
Student ID identifies the student.
Time buckets prevent unlimited row growth.
Salting helps distribute traffic.
Sparse columns allow different students to have different attributes.
Column families organize data according to access patterns.
HBase can distribute regions across multiple RegionServers.
This design is suitable for billions of records because it supports horizontal scaling and distributed storage.

Question 6
Discuss consistency and transactions in HBase and Cassandra. Why do distributed NoSQL databases often make different trade-offs from traditional ACID-based relational databases? Explain how consistency can affect application design.
Answer:
Consistency determines how quickly and predictably replicas reflect an update.
HBase generally provides strong row-level consistency and supports atomic operations at the row level.
Cassandra provides tunable consistency. It supports consistency levels such as:
ONE
QUORUM
ALL
Traditional relational databases generally provide ACID transactions:
Atomicity
Consistency
Isolation
Durability
Distributed NoSQL databases often prioritize:
Horizontal scalability
High availability
Low latency
Fault tolerance
Partition tolerance
Coordinating every transaction across multiple distributed nodes can increase network communication and latency. Therefore, NoSQL systems may provide narrower transaction scopes.
Application impact:
Critical operations may require stronger consistency, while analytics or logging applications may tolerate weaker consistency.
Applications should also handle:
Retries
Duplicate events
Delayed visibility
Partial failures
Therefore, consistency requirements should be selected according to the application's business needs.

Question 7
Availability and fault tolerance are important characteristics of distributed databases. Explain how Apache Cassandra achieves high availability using its distributed architecture. Discuss the role of replication and consistency levels in maintaining availability.
Answer:
Cassandra is designed as a highly available peer-to-peer distributed database.
There is no single master responsible for all data operations. Data is distributed across nodes and replicated.
Replication Factor:
Replication factor determines the number of copies of data.
For example:
Replication Factor = 3
means three replicas of the data are maintained.
Replication strategies:
SimpleStrategy
NetworkTopologyStrategy
NetworkTopologyStrategy is preferred for production multi-datacenter systems.
Consistency levels:
ONE:
Only one replica needs to respond.
QUORUM:
A majority of replicas must respond.
ALL:
All replicas must respond.
For example, if:
RF = 3
then QUORUM normally requires two replica responses.
Replication allows Cassandra to continue serving requests even if some nodes fail.
Therefore, Cassandra achieves high availability through:
Peer-to-peer architecture
Replication
Failure detection
Repair mechanisms
Tunable consistency

Question 8
Explain the major query features of HBase and Cassandra. Compare HBase operations with Cassandra's CQL (Cassandra Query Language). Why is query design and data modeling particularly important in column-family databases?
Answer:
HBase provides operations such as:
Get – Retrieve a row.
Put – Insert/update data.
Delete – Delete data.
Scan – Read a range of rows.
Filters – Filter returned data.
Cassandra provides CQL (Cassandra Query Language), which resembles SQL.
Common CQL commands include:
CREATE TABLE
INSERT
UPDATE
DELETE
SELECT
Example:
CREATE TABLE student_activity (
    student_id text,
    activity_date date,
    event_time timestamp,
    event_id uuid,
    event_type text,
    PRIMARY KEY ((student_id, activity_date), event_time, event_id)
);
Importance of data modeling:
Data is distributed according to keys.
Poor keys can create hotspots.
Cassandra queries should generally specify partition keys.
Large partitions can cause performance problems.
Joins are not the normal approach.
Data is often denormalized for faster queries.
Therefore, column-family databases should be designed according to expected queries and access patterns.

Question 9
Scaling is one of the major reasons for using NoSQL databases. Explain how HBase and Cassandra scale when data and user requests increase. Compare horizontal scaling, partitioning, replication, and load distribution in both systems.
Answer:
Both HBase and Cassandra primarily use horizontal scaling, meaning additional machines are added to the cluster.
HBase:
Tables are divided into regions.
Regions are assigned to RegionServers.
Large regions can split.
HMaster manages region assignment and balancing.
HDFS distributes data across DataNodes.
HDFS also provides replication.
Cassandra:
Data is distributed using token ranges.
Nodes are peers.
Data is replicated across nodes.
New nodes can be added to increase capacity.
Multi-datacenter deployment is supported.
Comparison:
Feature
HBase
Cassandra
Partitioning
Regions
Token ranges
Scaling
Add RegionServers/DataNodes
Add Cassandra nodes
Replication
Mainly through HDFS
Native Cassandra replication
Architecture
Master + RegionServers
Peer-to-peer
Load distribution
Region-based
Token/partition-based
Both databases can scale to very large datasets, but good key design is essential.

Question 10
An organization wants to build a real-time event logging system that generates millions of events per minute. Explain why a column-oriented NoSQL database may be suitable for this application. Design a conceptual data model and discuss the required consistency, availability, and scaling strategy.
Answer:
A column-family NoSQL database is suitable for real-time event logging because event records are usually:
High volume
Independent
Append-heavy
Distributed
Time-based
A Cassandra model could be:
Table: events_by_device_day

Partition Key:
(device_id, day_bucket)

Clustering Columns:
event_time, event_id

Other Columns:
event_type
source
payload
severity
location
metadata
Example:
(device101, 2026-10-06)
       |
       +-- 18:10:03 event001 LOGIN
       +-- 18:10:04 event002 CLICK
Consistency:
A moderate consistency level such as ONE or LOCAL_QUORUM can be selected depending on requirements.
Availability:
Use replication, for example:
Replication Factor = 3
Scaling:
Add nodes as traffic increases.
Use time-based buckets.
Avoid oversized partitions.
Monitor disk usage and latency.
Use appropriate partition keys.
This provides high write throughput, scalability and fault tolerance.

Question 11
Content Management Systems (CMS) and Blogging Platforms often contain articles, authors, tags, comments, metadata, and user activity. Explain how a column-family database can be used to design such systems. Discuss the challenges of handling varying and evolving data structures.
Answer:
A column-family database can store CMS data by grouping related information.
Example HBase table:
Table: Blog
Row Key: article_id
Column Families:
content
title
body
summary
author
author_id
author_name
metadata
category
language
publish_time
status
stats
views
likes
Comments can be stored separately:
Row Key = article_id#comment_id
Cassandra can also use query-specific tables such as:
articles_by_author
articles_by_tag
comments_by_article
activity_by_user
Challenges:
New attributes may be introduced.
Older records may not contain new fields.
Different articles may require different metadata.
Data consistency must be maintained.
Large comment collections may create large partitions.
Query patterns may change.
Therefore, flexible schema is useful, but proper naming, partitioning and query-oriented design are still necessary.

Question 12
Counters are frequently used in distributed applications for tracking page views, likes, downloads, API requests, and user activity. Explain how counters can be implemented in Cassandra. What problems can occur when counters are updated concurrently in a distributed environment?
Answer:
Cassandra provides a special counter data type for distributed counting.
Example:
CREATE TABLE page_views (
    page_id text PRIMARY KEY,
    views counter
);
To increase the counter:
UPDATE page_views
SET views = views + 1
WHERE page_id = 'P101';
Counters can be used for:
Page views
Downloads
API requests
Likes
User activity
Problems:
Duplicate events may cause over-counting.
Concurrent updates can arrive at different replicas.
Replicas may temporarily have different states.
Hot counters can receive extremely high traffic.
Counter operations have special restrictions.
Resetting counters is more complicated than updating normal columns.
For applications requiring highly accurate accounting, it can be better to store individual events with unique IDs and calculate aggregates separately.

Question 13
Explain the concept of expiring data (TTL – Time To Live) in Cassandra/HBase. Consider a system that stores temporary user sessions and automatically removes them after 24 hours. Explain how TTL can be useful and discuss its impact on storage management and application design.
Answer:
TTL (Time To Live) allows data to automatically expire after a specified period.
For example, 24 hours equals:
24 × 60 × 60 = 86400 seconds
A Cassandra operation can conceptually use:
INSERT INTO sessions (session_id, user_id, data)
VALUES ('S101', 'U10', '...')
USING TTL 86400;
After 24 hours, the value becomes expired.
HBase can also configure TTL at the column-family level.
Benefits:
Automatic removal of temporary data.
Reduces application-side deletion work.
Useful for sessions and temporary tokens.
Helps control storage growth.
Useful for security and privacy requirements.
Impact:
Expired data may not disappear physically at the exact expiration time because storage cleanup occurs through mechanisms such as compaction.
Heavy TTL usage can also increase:
Compaction
Disk I/O
Storage management overhead
Applications should therefore treat TTL as a data lifecycle mechanism, not as an exact real-time deletion mechanism.

Question 14
A company is developing a real-time analytics platform that must handle high write volume, large datasets, geographically distributed users, and occasional node failures. Compare HBase and Cassandra for this scenario and recommend one database. Justify your answer using architecture, consistency, availability, scalability, and query requirements.
Answer:
Both HBase and Cassandra can handle large datasets, but Cassandra is generally better for this particular scenario.
HBase:
Integrates with Hadoop and HDFS.
Provides strong row-level consistency.
Supports horizontal scaling.
Suitable for very large datasets.
Good choice when Hadoop integration is important.
Cassandra:
Peer-to-peer architecture.
High write throughput.
Highly available.
Supports multiple data centers.
Provides tunable consistency.
Designed for distributed applications.
Comparison:
Requirement
HBase
Cassandra
High write volume
Good
Excellent
Geographic distribution
Good
Excellent
Availability
High
Very high
Scalability
High
Very high
Multi-datacenter
Possible
Excellent
Consistency
Strong
Tunable
Hadoop integration
Excellent
Limited
Recommendation:
I would choose Apache Cassandra because the application requires:
High write volume
Geographic distribution
High availability
Fault tolerance
Horizontal scalability
Cassandra's peer-to-peer architecture and multi-datacenter replication make it particularly suitable.

Question 15
Column-oriented NoSQL databases provide scalability and flexibility, but they require careful data modeling. Critically analyze this statement using HBase and Cassandra as examples. Discuss how poor choices of partition keys, row keys, column families, replication and query patterns can negatively affect system performance.
Answer:
The scalability of HBase and Cassandra depends heavily on proper data modeling.
1. Poor partition keys:
In Cassandra, the partition key determines where data is stored. A partition key with very few values can create a hot partition.
In HBase, sequential row keys can cause writes to concentrate on one region, resulting in a hotspot.
2. Oversized rows or partitions:
Very large partitions can cause:
High memory consumption
Slow queries
Compaction problems
Increased latency
Time-based bucketing can help solve this problem.
3. Poor column-family design:
In HBase, too many column families can increase storage and operational overhead. Column families should be created according to data access and lifecycle requirements.
4. Excessive replication:
Replication improves availability but also increases:
Storage requirements
Network traffic
Repair overhead
Write cost
Therefore, replication should match application requirements.
5. Poor query patterns:
Queries involving full-table scans, cross-partition operations or inefficient filters can become expensive.
Cassandra works best when queries use the appropriate partition key.
6. Over-normalization:
Traditional relational normalization is not always appropriate in NoSQL databases. Data is often duplicated to support efficient queries.
Conclusion:
Column-family databases provide excellent scalability and flexibility, but they do not eliminate the need for database design.
Good choices of:
Row keys
Partition keys
Column families
Replication factor
Time buckets
Query patterns
lead to balanced load, predictable performance and efficient scaling.
Poor choices can result in hotspots, large partitions, high network traffic, compaction pressure and slow queries.
Final Conclusion
Apache HBase and Apache Cassandra are powerful column-family NoSQL databases designed for large-scale distributed applications. HBase is particularly useful when strong consistency and Hadoop/HDFS integration are important, while Cassandra is especially suitable for highly available, geographically distributed and high-write applications.
Successful implementation of either system depends heavily on careful data modeling, appropriate keys, partitioning, replication and query design.
