---
title: "Towards High Availability in YARN: Motivation and Proposed Solution"
date: "2013-01-19"
categories: 
  - "computing"
  - "projects"
  - "school"
tags: 
  - "apache"
  - "availability"
  - "big-data"
  - "distributed-computing"
  - "mysql-cluster"
  - "ndb"
  - "stateless"
  - "yarn"
---

Finally, it's the end of my 3rd semester with [EMDC](http://www.kth.se/en/studies/programmes/master/em/emdc "EMDC") and I would like to share our latest project: High Availability in YARN. This project is collaboration between EMDC and Swedish Institute of Computer Science ([SICS](http://www.sics.se/ "SICS")). The project members are Arinto (me :p) and [Mário](http://www.marioalmeida.eu/about/ "Mario Almeida"). Our project partners are Umit and Strahinja (they worked on node-manager of YARN). And this project is supervised by [Jim Dowling](http://www.jimdowling.info/ "Jim Dowling") and mentored by [Vasia Kalavri](http://twitter.com/vkalavri "Vasia's Twitter").

This post explains the motivation behind the project and our proposed solution. [The follow-up post](http://www.otnira.com/2013/02/03/ha-in-yarn-implementations-and-experiments/ "Towards High Availability in YARN: Implementations and Experiments") explains the implementations and experiments as proofs of concept of our solutions.

### **Problem statement**

YARN solves scalability issues of previous MapReduce framework. It also offers flexibility in executing the computation framework on top of a cluster where YARN is deployed (([Apache Hadoop YARN Background and Overview](http://hortonworks.com/blog/apache-hadoop-yarn-background-and-an-overview/ "Apache Hadoop YARN Background and Overview"))).  However, it still has one limitation, which is on its availability.

#### Why is this availability issue important?

In Distributed Computing, failure is a norm, which means YARN should have acceptable amount of availability. The importance is increased by the nature of MapReduce-type jobs. In production environment, MapReduce-type jobs are commonly executed in the magnitude of hours. It can be frustrating moment for data scientists and engineers that submit the jobs  when there are failures that can not be handled properly and the job progress is lost. The job needs to be submitted again and they lost the precious hours of job execution.

#### So, what's wrong with  YARN's availability?

To answer this question, let's take  a look at current YARN's architecture below. For more information on how YARN works, you can go to [Hortonwork's post](http://hortonworks.com/blog/introducing-apache-hadoop-yarn/ "Introducing Apache Hadoop YARN").

\[caption id="attachment\_587" align="aligncenter" width="584"\][![YARN Architecture](images/YARN-bw-med.png)](http://www.otnira.com/2013/01/19/towards-high-availability-in-yarn-motivation-and-proposed-solution/yarn-bw-med/) YARN Architecture\[/caption\]

 

Refer to figure above, **container and task failures are handled by node-manager**. When a container fails or dies, node-manager detects the failure event and launches a new container to replace the failing container and restart the task execution in the new container. **In the event of application-master failure, the resource-manager detects the failure and start a new instance of the application-master with a new container**. The ability to recover the associated job state depends on the application-master implementation. MapReduce application-master has the ability to recover the state but it is not enabled by default. Other than resource-manager, associated client also reacts with the failure. The client contacts the resource-manager to locate the new application-master’s address.

**Upon failure of a node-manager, the resource-manager updates its list of available node-managers**. Application-master should recover the tasks run on the failing node-managers but it depends on the application-master implementation. MapReduce application-master has an additional capability to recover the failing task and blacklist the node-managers that often fail.

**Failure of the resource-manager is severe since clients can not submit a new job and existing running jobs could not negotiate and request for new container**. Existing node-managers and application-masters try to reconnect to the failed resource-manager. The job progress will be lost when they are unable to reconnect. This lost of job progress will likely frustrate engineers or data scientists that use YARN because typical production jobs that run on top of YARN are expected to have long running time and typically they are in the order of few hours. Furthermore, this limitation is preventing YARN to be used efficiently in cloud environment (such as Amazon EC2) since node failures often happen in cloud environment.

### **Proposed Solution**

Apache's solution to this issue is to use recovery failure model ((In this recovery failure model, new resource-manager will be started to replace the failed resource manager. The new resource-manager should have same address and port as the failed resource-manager. The proposed recovery failure model is transparent to clients, that means clients does not need to re-submit the jobs. In this model, resource-manager saves relevant information upon job submission)) using ZooKepeer or HDFS-based storage to store resource-manager's states. However, recovery failure model has some drawbacks which are existence of downtime when starting new resource-manager and unsuitability of HDFS to handle many small files (i.e. YARN application states).

Our proposal is to use stateless failure model using MySQL Cluster-based storage. We will explain our solution in section Stateless Failure Model and MySQL Cluster.

#### Stateless Failure Model

In this failure model, all necessary information and states used by resource-manager are stored in a persistent storage. Based on our observation, these information include:

1. Application related information such as application-id, application-submission-context and application-attempts.
2. Resource related information such as list of node-managers and available resources.

Figure below shows the architecture of stateless failure model.

\[caption id="attachment\_591" align="aligncenter" width="550"\][![Stateless Failure Model](images/Stateless-bw-med.png)](http://www.otnira.com/2013/01/19/towards-high-availability-in-yarn-motivation-and-proposed-solution/stateless-bw-med/) Stateless Failure Model\[/caption\]

Since all the necessary information are stored in persistent storage, it is possible to have more than one resource-managers running at the same time. All of the resource-managers share the information through the storage and none of them hold the information in their memory. When a resource-manager fails, the other resource-managers can easily take over the job since all the needed states are stored in the storage. Clients, node-managers and application-masters need to be modified so that they can point to new resource-managers upon the failure.

To achieve high availability through this failure model, we need to have a storage that has these following requirements:

1. The storage should be highly available. It does not have single-point-of-failure.
2. The storage should be able to handle high read and write rates for small data (in the order of at most few kilo bytes), since this failure model needs to perform very frequent read and write to the storage.

ZooKeeper and HDFS satisfy the first requirement, but they do not satisfy the second requirement. ZooKeeper is not de signed as a persistent storage for data and HDFS is not de signed to handle high read and write rates for small data. We need other storage technology and MySQL Cluster (NDB) is suitable for these requirements.

#### MySQL Cluster (NDB)

MySQL Cluster (NDB) is a scalable in-memory distributed database. It is designed for availability, which means there is no single-point-of-failure in NDB cluster. Furthermore, it complies with ACID-transactional properties. Horizontal scalability is achieved by auto-data-sharding based on user-defined partition key. The latest benchmark from Oracle shows that MySQL Cluster version 7.2 achieves horizontal scalability, i.e when number of datanodes is increased 15 times, the throughput is increased 13.63 times (([MySQL Cluster 7.2 GA Released, Delivers 1 BILLION Queries per Minute](http://dev.mysql.com/tech-resources/articles/mysql-cluster-7.2-ga.html "My SQL Cluster 7.2"))).

Regarding the performance, NDB has fast read and write rate. The aforementioned benchmark shows that 30-node-NDB cluster supports 19.5 million writes per second (([MySQL Cluster 7.2 GA Released, Delivers 1 BILLION Queries per Minute](http://dev.mysql.com/tech-resources/articles/mysql-cluster-7.2-ga.html "My SQL Cluster 7.2"))). It supports fine-grained locking, which means only affected rows are locked during a transaction. Updates on two different rows in the same table can be executed concurrently. SQL and NoSQL interfaces are supported which makes NDB highly flexible depending on users’ needs and requirements.

Figure below shows the high level diagram of the proposed architecture. NDB is introduced to store resource-manager states.

[![YARN wiith MySQL Cluster NDB](images/YARN-ndb-bw2-med.png)](http://www.otnira.com/2013/01/19/towards-high-availability-in-yarn-motivation-and-proposed-solution/yarn-ndb-bw2-med/)

 

In our next post, we will share our implementations and experiments as proofs of concept of our solutions. Stay tuned!

Here is the [follow up post!](http://www.otnira.com/2013/02/03/ha-in-yarn-implementations-and-experiments/ "Towards High Availability in YARN: Implementations and Experiments")
