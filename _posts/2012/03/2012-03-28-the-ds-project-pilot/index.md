---
title: "Large-Scale Decentralized Storage Systems for Volunter Computing Systems - Pilot"
date: "2012-03-28"
categories: 
  - "computing"
  - "school"
tags: 
  - "distributed-computing"
  - "distributed-data-storage"
  - "ds"
  - "volunteer-computing"
---

Well, I would like to write something about our Decentralized System (DS) project and what we are going to do in this project. Since it is the first post about the big picture of our DS project in this blog, I named it "Pilot" :p. Our group is G007, which consists of [Julia](http://blog.proskurnia.in.ua/), Diego, Enkhjin and myself.

After two weeks of paper reading and some brainstorming sessions, we are proposing these following directions for our project, **Large-Scale Decentralized Storage Systems for Volunter Computing Systems**.

**Background Information**

First of all, we would like to discuss about **Volunter Computing (VC) Systems.** So what is VC? Well, I've explained it before in my previous [post](http://otnira.wordpress.com/2012/03/12/volunteer-computing/). But here, we would like to re-highlight its characteristics, which are:

1. VC consists of group of computing resources, and each node in VC **voluntary** shares its resources (CPUs or storage) to achieve common goals (such as [analyzing radio telescope data to find extraterrestrial intelligence](http://setiathome.berkeley.edu/)). Usually the type of task that executed in VC belongs to [Embarrassingly parallel](http://en.wikipedia.org/wiki/Embarrassingly_parallel) category, which can be easily divided into independent parallel task.
2. The participating nodes **are not able to continuously connected** or available to the system.
3. This kind of system is built on **trust** between nodes.
4. It is **highly dynamic**, which means it has no guarantee how many participating machines will be available at given time or how long a given resource will be available in the system.
5. Participating nodes are given some **incentives**. And the type of incentives really depend on the participating nodes, some participants enjoy seeing application graphics, either in a window or as a screensaver. Others are motivated by competition with respect to computer speed and donated CPU time.

Nowadays, **storage in VC is centralized**. The participating nodes fetch the data from the central storage server or the centralized controllers send the data to the participating nodes. __**This characteristic reduces the scalability of VC since there may be bottleneck on the storage server bandwidth and processing power.**__

\[caption id="" align="aligncenter" width="385" caption="Storage in VC is centralized and may introduce bottleneck"\]![Volunteer Computing Architecture](images/VolunteerComputingArchv2.png "Volunteer Computing (VC) Architecture")\[/caption\]

Now, comes the second element of this project, which is **Distributed Storage System (DSS).** For the definition, well, it is rather self-explained. It is storage system that distributed among networks of computer. The main points of these kind of systems are **no single point of failure** and **scalability** while maintaining **data availability and integrity**. Example of these systems are [Cassandra](http://cassandra.apache.org/) (used by Facebook), [Voldemort](http://project-voldemort.com/) (used by LinkedIn). Both of these example are using dedicated storage resources. Another type of this system is based on Peer-to-Peer technologies as presented in these following survey paper, titled ["A Survey of Peer to Peer Storage Techniques for Distributed File Systems"](http://ieeexplore.ieee.org/xpl/freeabs_all.jsp?arnumber=1425146). Peer-to-Peer technology means the later they do not really needs dedicated resources and expected to have high scalability.

**The Challenges and Proposal**

Centralized storage system in Volunteer Computing **reduces the scalability of VC** and **introduces** bottleneck on the storage server bandwidth and computing power. And until now, Distributed Storage System for VC is **not really exist yet**, although research in DSS is abundance and more hardware capability to perform storage function.

Therefore, having **DSS as replacement of centralized storage server in VC can improve the scalability and remove the bottleneck on the storage system**. This concept is depicted in figure below

\[caption id="" align="aligncenter" width="517" caption="DSS for VC high level architecture"\]![DSS for VC high level architecture](images/VolunteerComputingDecenv2.png "DSS for VC")\[/caption\]

**The Project Objectives**

Given the above challenges and proposal, we would like to perform a **survey** based on several **metrics** (such as Availability, Reliability, and Security) that important for Volunter Computing. Then, if the time permits, we will perform **performance analysis** of best DSS implementation based on our metrics. We plan to use one of the available DSS implementation, and observe its performance using distributed system testbed such as [PlanetLab](http://www.planet-lab.org/).

**Work Done**

Until last week of March, we have read many papers. I will update this blog with summary of what we have reading, or update this post with the appropriate link (since some of the group members posted the review in their blog or Google Docs). Other than summary, currently we also constructing a spreadsheet that define the characteristics of DSS that we reviewed. The characteristics includes our observation about their suitability for Volunteer Computing. I will share the link once the spreadsheet is finished.

DSS that has been reviewed (will be updated along the way):

1. [Squirrel](http://blog.proskurnia.in.ua/post/19303300782/decentralized-web-cache)
2. [Storage@Home](http://blog.proskurnia.in.ua/post/19236912837/volunteer-computing)
3. [P2PTuple](http://blog.proskurnia.in.ua/post/19236912837/volunteer-computing) (scroll to the middle of the post)
4. [Farsite](http://blog.proskurnia.in.ua/post/18919619835/review-distributed-file-systems), another review is [here](https://docs.google.com/document/d/1irJhGveCyhQUMHnsQG-sBCpQP5A_rTkOdue78c0wQyo/edit)
5. [Ivy](http://blog.proskurnia.in.ua/post/18919619835/review-distributed-file-systems) (scroll to the middle or use control F)
6. [Cassandra](http://blog.proskurnia.in.ua/post/18919619835/review-distributed-file-systems)(scroll to the middle or use control F)
7. [Glacier](http://blog.proskurnia.in.ua/post/18919619835/review-distributed-file-systems)
8. [TFS](https://docs.google.com/document/d/1s_lwj-fb03ftDTMxfyN4vNkEnvKs-_N7jALxMfUaYaY/edit)
9. [Pastis](https://docs.google.com/open?id=0ByPFxaSlC14zQm5hY0VBYmdUQ2UyLUpKaUVoV25oZw)
10. [PAST](https://docs.google.com/document/d/14fa7AVJBHmCBK0yAQYc4iPXnfoT7bwdd4mAfy1-2l6U/edit)
11. [Dynamo](https://docs.google.com/document/d/1x4Gv56Kjfrz-NKYTgmy0mSQEUMi_ZyhVn5Jc-LeXgcA/edit)
12. [Riak](https://docs.google.com/document/d/1mjpp9EryrXTcR1Ntvpta770UwntBnzl8SIXnX5Rpu8o/edit)
13. [OceanStore](https://docs.google.com/open?id=0ByPFxaSlC14zOEg2bUdGTHBUNnFZTFpuSEtQRXZWQQ)
14. [Wuala](http://otnira.wordpress.com/2012/03/25/wuala-p2p-distributed-storage/)
15. [TotalRecall](https://docs.google.com/document/d/13dqjq_qD6q-gtbuBeCBaFtA-Wws3fmpFWkhw9-wXncg/edit)
16. [Voldemort](https://docs.google.com/document/d/1QZB9umfxSfqreYYSGjelncwUjS-xk4r0oAxmOloUtSM/edit)
17. [Attic](https://docs.google.com/document/d/1TSaxOOpAudCIm7f5kuTLIDcJTOCvVecZuCYQSU6-eus/edit)
18. [Kademlia](https://docs.google.com/document/d/1wUER7N8PqF1C4_12jY-mdDxbEGsU6e7UG44smHpoAM0/edit) (Overnet backbone)

The slides that used for our project presentation can be found here: http://www.slideshare.net/arinto/distributed-storage-system-for-volunteer-computing
