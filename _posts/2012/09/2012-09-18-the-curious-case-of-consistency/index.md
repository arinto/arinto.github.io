---
title: "The Curious Case of Consistency - Part 1"
date: "2012-09-18"
categories: 
  - "computing"
tags: 
  - "amazon-s3"
  - "azure"
  - "cap"
  - "consistency"
  - "distributed-computing"
---

Last week I attended interesting breakfast talk by [Doug Terry](http://research.microsoft.com/en-us/people/terry/), principal researcher from Microsoft Research Silicon Valley and I think it is pretty cool talk B-). The talk itself is about other consistency types which lie between Strong and Eventual Consistency, and how the additional consistency types are (practically) used through simple pseudo-code of baseball game. And this is the first of two planned posts for this topic.

Let's start with two most basic consistency model (which we usually learn in our first Distributed System course/module in University): **Strong Consistency** and **Eventual Consistency**.

In **Strong Consistency**, every node in distributed system will **always** see the same and latest view after a specific node update the data. This consistency type sacrifices performance and availability in order to ensure consistent view across the node in the distributed system since it is expensive in term of network and computing resources needed to maintain the consistency. Example of system that uses Strong Consistency is Windows Azure.  Strong Consistency is desirable within a data center however performance and availability concerns start raising in geo-replication service that spans in multiple-data center.

\[caption id="attachment\_455" align="aligncenter" width="500"\][![](images/windows_azure_logo-e1348003276446.jpg "windows_azure_logo")](http://www.otnira.com/wp-content/uploads/2012/09/windows_azure_logo-e1348003276446.jpg) I use strong consistency!\[/caption\]

**Eventual Consistency** favors performance and availability rather than consistency itself. This means nodes in distributed system are not guaranteed to see the latest view after data updates (in other words, nodes may see stale data), but **eventually** they will see the latest data. Example of system that uses this consistency type is Amazon Simple Storage Service (S3).

\[caption id="attachment\_456" align="aligncenter" width="399"\][![](images/Amazon-AWS-plus-S3-logo_scaled.png "Amazon AWS plus S3 logo_scaled")](http://www.otnira.com/wp-content/uploads/2012/09/Amazon-AWS-plus-S3-logo_scaled.png) I use eventual consistency!\[/caption\]

Practically, the choice of consistency model depends on type of the application and it will introduce trade-off between consistency, performance and availability. It is interesting to note that the trade-off is not between consistency, availability and partition tolerance as in CAP theorem. It seems that aforementioned Doug's argument on the trade-off supports the idea of moving on from CAP theorem as discussed in [this previous post](http://www.otnira.com/2012/04/21/consistency-tradeoff-in-modern-distributed-db/ "Consistency Tradeoff in Modern Distributed DB").

Now, let's see what are the consistency models that are discussed during the talk. First, we need to define what the data model that used in this consistency model discussion is. Quoting from Doug's paper: "_The data model is based on simple model where clients read and write data into a data-store. The data is replicated among a set of servers, but the details of the replication protocol are hidden from clients. Writes are serialized and eventually performed in the same order at all servers. This order is consistent with order in which write operations are submitted by clients. Reads return the values of one or more data objects that were previously written, though not necessarily the latest values. Each read operation can request a consistency guarantee, which dictates the set of allowable return values. Each guarantee is defined by the set of previous writes whose results are visible to a read operation_"\[1\]

Based on the aforementioned data model, **Strong Consistency** will ensure that each client will see all previous writes. **Eventual Consistency** is the weakest among the other consistency types. It has the most number of set of possible read result as return value. Then, we have **Consistent Prefix**, which is similar to "snapshot isolation" feature of some database management system. In this model, a reader is guaranteed to see a version of data that existed in master node at some time at the past. The reader sees a valid ordered sequence of writes since the first write to the data object under interest. **Bounded Staleness** guarantees that a reader sees valid data up to T unit of time from now into the past, i.e. reader will see valid data that is written more than T minutes ago. Staleness can also be defined as the number of missing writes or the inaccuracy amount of data value. **Monotonic Reads** ensures that the read data is always more updated or at least same freshness compared to the latest read. **Read My Writes** ensures that each client will always see the changes into the data that he has written. Note that in this post, we do not order the consistency based on its strength but we put four additional consistency types in between Strong Consistency and Eventual Consistency.

These two tables from Doug's paper\[1\] shows the summary of all the consistency techniques

[![](images/Table1.png "Six consistency types")](http://www.otnira.com/wp-content/uploads/2012/09/Table1.png)

[![](images/Table2.png "Matrices")](http://www.otnira.com/wp-content/uploads/2012/09/Table2.png)Now, comes the interesting part, which is implementing it in a baseball game and I'll cover it on the next post.. :)

#### References:

_\[1\] D. Terry, “Replicated Data Consistency Explained Through Baseball,” Oct-2011. \[Online\]. Available: [http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf). \[Accessed: 18-Sep-2012\]._
