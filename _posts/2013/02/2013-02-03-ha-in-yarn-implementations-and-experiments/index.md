---
title: "Towards High Availability in YARN: Implementations and Experiments"
date: "2013-02-03"
categories: 
  - "computing"
  - "projects"
  - "school"
tags: 
  - "availability"
  - "clusterj"
  - "distributed-computing"
  - "hadoop"
  - "high-availability"
  - "mapreduce"
  - "mysql"
  - "ndb"
  - "yarn"
---

This post is a follow-up post about our project, High Availability in YARN. In the [previous post](http://www.otnira.com/2013/01/19/ha-in-yarn-motivation-and-proposed-solution/ "Towards High Availability in YARN: Motivation and Proposed Solution"), we have explained the motivation and our proposed solution to solve availability problem in YARN. Now, let's continue with the implementations and experiments that we have done as proofs of concepts for our proposed solution.

### **Implementation**

As a proof-of-concept of our proposed architecture, we designed and implemented NDB storage module for YARN resource-manager. Due to limited time, recovery failure model was used in our implementation. In this post, we will refer the proof-of-concept of NDB-based-YARN as YARN-NDB.

We designed two NDB tables to store application states and their corresponding application-attempts. They are called _applicationstate_ and _attemptstate_. Figures below shows the columns in both tables.

\[caption id="attachment\_630" align="aligncenter" width="302"\][![Columns in applicationstate table](images/ApplicationState-Table.png)](http://www.otnira.com/2013/02/03/towards-high-availability-in-yarn-implementations-and-experiments/applicationstate-table/) Columns in applicationstate table\[/caption\]

\[caption id="attachment\_631" align="aligncenter" width="312"\][![Columns in applicationattempt table](images/ApplicationAttempt-Table.png)](http://www.otnira.com/2013/02/03/towards-high-availability-in-yarn-implementations-and-experiments/applicationattempt-table/) Columns in applicationattempt table\[/caption\]

 

To enhance table performance in term of read and write throughput, partitioning technique was used (([MySQL Partitioning](http://dev.mysql.com/doc/refman/5.5/en/partitioning-key.html "MySQL Partitioning"))). Both tables were partitioned by applicationid and clustertimestamp. With this technique, NDB located the desired data without contacting NDB's location resolver service, hence it was faster compared to NDB tables without partitioning.

We developed YARN-NDB using ClusterJ (([Chapter 4. MySQL Cluster Connector for Java](http://dev.mysql.com/doc/ndbapi/en/mccj.html "ClusterJ"))) . Our final work is based on YARN-231-2 patch (([YARN-231: Add FS-based persistent store implementation for RMStateStore](https://issues.apache.org/jira/browse/YARN-231 "YARN 231"))) on top of Hadoop trunk dated 23 December 2012. The NDB storage module in YARN-NDB has same functionalities as Apache YARN’s HDFS and ZooKeeper storage module such as adding and  deleting application states and attempts. We also developed unit test module for the storage module, check this [flowchart](http://www.otnira.com/wp-content/uploads/2013/02/HAinYARN-flowchart2.png "Unit Test Flowchart") if you are interested on the unit test flows.

### **Evaluation: NDB Storage Module Correctness**

The evaluation using the aforementioned unit test is performed using single-node-NDB-cluster (two NDB datanode-process in a node) on top of a computer with 4GB RAM and Intel dual-core i3 CPU at 2.40 GHz. The unit test was executed using Maven and the result was positive. Our NDB storage module passes the unit test. We followed up to test its consistency by executing unit test several times and the results were always pass.

After unit test, we continued with actual resource-manager failure test. In this test, we used SICS' machines with 30GB RAM and two six-core AMD Opteron processor at 2.6 GHz in each of them. Ubuntu 11.04 with Linux Kernel 2.6.38-12-server was installed as the operating system and Java(TM) SE Runtime Environment (JRE) version 1.6.0 was the Java Runtime Environment.

NDB was deployed in 6-node-cluster and YARN-NDB was configured using single-node setting. We executed _pi_ and _bbp_ examples that come from Hadoop distribution. In the middle of pi and bbp execution, we terminated the resource-manager process using Linux _kill_ command. The new resource-manager with the same address and port was started three seconds after the old one was successfully terminated.

We observed that the  new resource-manager that started using same address was able to continue the submitted jobs (pi and bbp). Several connection-retry-attempts to contact resource-manager are observed. These retry attempts were initiated by node-managers, clients and application-masters. To check for consistency, we submitted a new job to the new resource-manager and the new job was finished correctly. We repeated this experiment several times and same results were observed, i.e the new resource-manager was successfully restarted and took over the killed resource-manager’s roles correctly.

### **Evaluation: NDB Performance Evaluation**

In this evaluation, we compared NDB performance with Apache's proposed storage modules: ZooKeeper and HDFS. We developed a benchmarking framework, [zkndb](https://github.com/4knahs/zkndb "zkndb"). For more details about zkndb framework, please refer to its corresponding wiki pages. In nutshell, we measured throughput of NDB, ZooKeeper and HDFS under three types of workload

1. Read-intensive, one set of data was written into the database and zkndb always read on written data.
2. Write-intensive, no read was performed and zkndb always wrote a new set of data into different location.
3. Balanced read-and-write, read and write were performed alternately.

Each data-write into the storage had an application identification and application state information. Application identification was a Java long data type with size of eight bytes. Application state information was an array of random bytes, with length of 53 bytes. The length of application state information was determined after observing actual application state information that stored when executing YARN-NDB jobs. Each data-read consisted of reading an application identification and its corresponding application state information.

We varied the throughput rate and we investigated the scalability and performance of each storage. Figure below shows the throughput for 8 threads with 1 minute of benchmark execution time.

\[caption id="attachment\_645" align="aligncenter" width="512"\][![zkndb Throughput Benchmark Result for 8 Threads and 1 Minute of Benchmark Execution ](images/8-1000-60000-0.jpeg)](http://www.otnira.com/wp-content/uploads/2013/02/8-1000-60000-0.jpeg) zkndb Throughput Benchmark Result for 8 Threads and 1 Minute of Benchmark Execution\[/caption\]

For all three workload types, NDB had the highest throughput compared to ZooKeeper and HDFS. These results can be attributed to the nature of NDB as a high performance persistent storage which is capable to handle high read and write request rate. Refer to the error bar in figure above, NDB has big deviation between its average and the lowest value during experiment. This big deviation could be attributed to infrequent intervention from NDB management process to recalculate the data index for fast access.

Interestingly, ZooKeeper’s throughput were stable for all workload types. This throughput stability can be accounted to ZooKeeper’s behavior to linearize incoming requests that causes read and write request have approximately the same execution time. Another possible explanation for ZooKeeper’s throughput stability is the YARN’s ZooKeeper storage module implementation. The module implementation code could cause the read and write execution time equal.

As expected, HDFS had the lowest throughput for all work load types. HDFS’ low throughput may be attributed to HDFS’ NameNode-locking overhead and inefficient data access pattern when it processes lots of small files. Each time HDFS receives read or write request, HDFS NameNode needs to acquire a lock for the file path so HDFS can return a valid result. Acquiring a lock frequently increases data access time hence decreases the throughput. The inef ficient data access pattern in HDFS is due to data splitting to fit the data into HDFS block and data replication. Furthermore, the needs to write the data into disk in HDFS decreases the throughput as observed in write-intensive and read-write balance workload.

Two figures below show the scalability results of the databases under test.

\[caption id="attachment\_649" align="aligncenter" width="500"\][![Scalability Results for Read-Intensive Workload](images/results-01.png)](http://www.otnira.com/wp-content/uploads/2013/02/results-01.png) Scalability Results for Read-Intensive Workload\[/caption\]

\[caption id="attachment\_650" align="aligncenter" width="500"\][![Scalability Results for Write-Intensive Workload](images/results-10.png)](http://www.otnira.com/wp-content/uploads/2013/02/results-10.png) Scalability Results for Write-Intensive Workload\[/caption\]

 

All of the storage implementation increased their throughput when the number of threads were increased. NDB had the highest increase compared to HDFS and ZooKeeper. For NDB, doubling the number of threads increased the throughput by 1.69 for read-intensive workload and 1.67 for write-intensive workload. These results were close to linear scalability.

On the other hand, write-intensive-workload HDFS performed very poor for this workload. The highest throughput achieved by NDB with 36 threads was only 534.92 requests per second. The poor performance of HDFS could be attributed to the same reasons as explained before, which are NameNode-locking overhead and inefficient data access pattern for small files.

### **Conclusion**

We have presented an architecture for highly-available cluster computing management framework. The proposed architecture incorporated state-less failure model into existing Apache YARN. To achieve the high-availability nature and the state-less failure model, MySQL Cluster (NDB) was proposed as the storage technology for storing the necessary state information.

As a proof-of-concept, we implemented Apache YARN's recovery failure model using NDB (YARN-NDB) and we developed zkndb benchmark framework to test it. Availability and scalability of the implementation has been examined and proven using unit test, actual resource-manager failure test and throughput benchmark experiments. Results showed that YARN-NDB was better in term of throughput and ability to scale compared to existing ZooKeeper and HDFS-based solutions.

For further reference, here are the formal report and the slides that I used for final project presentation.

http://www.slideshare.net/arinto/next-generation-hadoop-high-availability-for-yarn

http://www.slideshare.net/arinto/high-availability-in-yarn
