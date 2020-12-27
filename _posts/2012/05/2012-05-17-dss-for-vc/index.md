---
title: "Large-Scale Decentralized Storage Systems for Volunter Computing Systems – Final"
date: "2012-05-17"
categories: 
  - "computing"
  - "school"
tags: 
  - "decentralized-storage-system"
  - "volunteer-comptuing"
---

This is my first post in the new domain :). And I would like to present the final report and presentation of Decentralize System (DS) project, which I posted the overview [here](http://www.otnira.com/2012/03/28/the-ds-project-pilot/).

We continued with the survey of Decentralized Storage Systems(DSS), and we ranked them based on these five following characteristics

1. **Availability (AV)**. Formal definition is fraction of system uptime and able to perform normally over the uptime + downtime. In the DSS context, it is usually reflected as degree of system resistance of churn and fault tolerance level that implemented by the system, and how easy and fast we can get file or data that we want.
2. **Scalability (SC)**. Ability of the system to be enlarged  to accommodate its growth. In the context of DSS, it could be in term of number of nodes, number of messages exchanged, number of data stored in the storage system.
3. **Eventual Consistency (ECO)**. ECO is prefered than C, because Availability is more preferred and pure Consistency is not practical and expensive to achieve in term of required messages between nodes.
4. **Performance (P)**. Example of metrics that can be used to measure performance are response time to satisfy search request. We analyze whether the system put emphasize on it or not.
5. **Security (SE)**. Resistance into some degrees of attack or breach such as compromising data integrity and unauthorized data access by malicious node. Due to the nature volunteer computing, security is one of the important feature to attract volunteer and ensure data validity

We also identified the focus for each DSS that we survey as shown below. It is ordered from least suitable for VS to the most suitable.

\[caption id="" align="aligncenter" width="590" caption="DSS Classification for VS"\]![DSS Classification for VS](images/Rank.PNG "DSS Classification for VS")\[/caption\]

 

We propose state-of-the-art system for DSS in Volunteer Computing environment as shown in figure below. First of all, **Read and Write access**, in which we assume that our system needs far more read access than write access because VC nodes will not be updating any files. Furthermore, **Fault Tolerance and Replication** techniques could be achieved by Byzantine Fault Tolerance and smart replication techniques that are based on geographic locality and popularity based replicas. This also ensures good availability. In consideration of **Symmetry and Availability**, we found that although symmetry among PSP nodes is desirable, the system as a whole does not need to be symmetric.  One of the interesting replication techniques would be erasure coding, and fragmentation of files. Finally, a strong incentives would be a good idea. 

 

\[caption id="" align="aligncenter" width="574" caption="State-of-the-art DSS for VC"\]![State-of-the-art DSS for VC](images/PerfectSystem.png "State-of-the-art DSS for VC")\[/caption\]

 

The use of incentives in the distributed storage system nodes, as well as the volunteers in the volunteer computing system, is an effective way to increase and maintain highly committed members. **Incentives based on credit system**, in which individuals are granted credits and certificates for their contribution of computing powers, have proven very effective

These type incentives encourage users to compete among each other with their credits, thus contributing more towards the computing system. Similarly, a credit and certificate based on amount of storage shared, and available time in the system would contribute towards a highly available distributed storage system. Government may also provide tax-break for volunteers that participating in government projects.

We also analyze the **challenges** and I think the most interesting challenges is i**ntegration of chosen DSS or state-of-the-art DSS into existing Volunteer Computing environment**. It is necessary to have minimal impact on current BOINC system since it is production system with hundreds of thousands of users.

To solve this challenge, introduction of proxy solution to interface both sides may be useful.We may also need to introduce separate layer between VC participants that contribute computing resources and storage resources. These layers will provide several API as interface hence we will have flexibility in interchanging the implementation of storage layer.

Here are the deliverables of  this project  (slides and report) ! Enjoy!! :D

http://www.slideshare.net/arinto/group7-presentationfinal

http://www.slideshare.net/arinto/ds-g7-survey
