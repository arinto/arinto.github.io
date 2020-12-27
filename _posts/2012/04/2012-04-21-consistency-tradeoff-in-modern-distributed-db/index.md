---
title: "Consistency Tradeoff in Modern Distributed DB"
date: "2012-04-21"
categories: 
  - "computing"
  - "school"
tags: 
  - "cap"
  - "distributed-computing"
  - "distributed-data-storage"
  - "pacelc"
  - "sds"
---

Last week I had presentation about the relevancy of CAP theorem in modern distributed system design. This presentation is based on an article titled "[Consistency Tradeoffs in Modern Distributed Database System Design](http://doi.ieeecomputersociety.org/10.1109/MC.2012.33 "Abadi's Paper")" by Daniel J. Abadi from Yale University.

CAP theorem is widely used in Distributed Database System(DDBS) design. In a nutshell, it says that in designing modern DDBS, we only can choose two properties out of three properties that are crucial for DDBS. The aforementioned properties are Consistency (C), Availability (A) and Partition Tolerance (P).  And this diagram below summarize the available combination of CAP:

\[caption id="" align="aligncenter" width="366"\]![CAP Diagram](images/CAP.PNG "CAP Diagram") CAP Diagram\[/caption\]

Now, the question here are**, is there something wrong with CAP theorem? Is it still relevant with modern DDBS design?** Well, the answer for second question is yes, of course, it is still relevant **BUT** the answer for the first question is also yes! CAP has a flaw in explaining why modern DDBSs reduce consistency.

Well, according to CAP theorem, consistency is reduced because

1. DBSS must have Partition Tolerance (P)
2. High Availability (A) is desired or part of the requirements of the database.

The next question is "**what does partition tolerance really mean?**"

The P  in CAP has two elements

1. The partition tolerance itself ~ commonly used as justification in reducing consistency
2. The **existence** of network partition itself ~ often forgotten in justification

The second elements of CAP leads to following question:

\[caption id="" align="aligncenter" width="320"\]![No network partition, why consistency is still reduced?](images/Philosoraptor.jpg "No network partition, why consistency is still reduced?") No network partition, why consistency is still reduced?\[/caption\]

To answer [Philosoraptor](http://knowyourmeme.com/memes/philosoraptor) question above, we need to revisit what are the design goals of modern DDBSs. It turns out to be **Availability and Latency**, as we all see in [Dynamo](https://docs.google.com/document/d/1x4Gv56Kjfrz-NKYTgmy0mSQEUMi_ZyhVn5Jc-LeXgcA/edit) (used by Amazon), [Cassandra](https://www.facebook.com/note.php?note_id=24413138919) (used by Facebook), [Voldemort](http://project-voldemort.com) (used by LinkedIn) and [PNUTS](http://research.yahoo.com/project/212) ( used by Yahoo). To achieve Availability, they will use replica. And unfortunately, when replication is used, tradeoff between Latency and Consistency starts to arise!

To capture Consistency/Latency tradeoff, the writer proposes new metrics to be used in modern DBSS design which is called **PACELC**. It consists of two element PAC and ELC. PAC means when there is **Partition**, tradeoff between **Availability** and **Consistency** occurs. And ELC means, **Else** (when there is no Partition), tradeoff between **Latency** and **Consistency** occurs. Lastly, table below shows the classification of modern DBSS using the new metrics:

\[caption id="" align="aligncenter" width="499"\]![DBSS classification using PACELC](images/Metrics.PNG "DBSS classification using PACELC") DBSS classification using PACELC\[/caption\]

Overall, I believe that CAP theorem is still important for designing modern DB system. We should not abandon it. Exploring other metric to keep up to date with latest technology and consideration is good practice. And I think PACELC is worth to consider and applied in designing modern DB system.

Here is the slide that I used during presentation:

http://www.slideshare.net/arinto/consistency-tradeoffinmoderndb
