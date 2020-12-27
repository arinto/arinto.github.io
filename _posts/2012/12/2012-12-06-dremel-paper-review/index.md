---
title: "Dremel - Paper Review"
date: "2012-12-06"
categories: 
  - "computing"
  - "school"
---

Time is really ticking and somehow this semester I do not able to post as often as last semester.. Well, let's start posting again.. hehe

I did paper review on [Dremel](http://research.google.com/pubs/pub36632.html "Dremel") (or [here](http://dl.acm.org/citation.cfm?id=1920886 "Dremel ACM") for ACM version) as part of ID2220 (Advanced Topics in Distributed System assignment) and here is the summary of my review. I also attached very nice slides on Dremel done by my classmate, [Maria](http://www.linkedin.com/in/marsty5 "Maria's LinkedIn"), at the end of this post.

### What is Dremel?

Data analytics platform that allows interactive/ad-hoc exploration for web-scale data sets.

### Why do we need Dremel?

Google's MapReduce-BigTable and Hadoop at that time were too slow for interactive exploration. Furthermore, they are not really user-friendly for users who already familiar to SQL.

### So, how does Dremel work?

Two important mechanisms in Dremel that allow it to solve the ad-hoc query problem are:

1. It transforms record-structure data model into columnar data model. Columnar data model only contains data/fields that are needed for ad hoc query. This data model suits ad-hoc query well because typical ad hoc query does not need all the columns/field in a specific table.
2. It uses multi-level serving tree to process the columnar data model. The tree structure allows high degree parallelism in processing the data and it is suitable for small or medium-size data typically found in ad hoc queries.

### What are strong points of Dremel paper?

1. Identification of ad-hoc queries dataset allows development of appropriate of efficient methods to tackle them.
2. Fast and lossless conversion between record structure to columnar data model
3. The authors used real-world Google workload and data sets. This real-world data usage is very good and improve the confirming power of Dremel.

### What are the weak points of Dremel paper?

1. Record oriented data model can still outperform columnar data model, which is when the ad hoc queries need lots of column.
2. No discussion on the perfromance of data model conversion.
3. "Cold" setting usage in Local Disk experiment, which make the results are dominated with disk access time.

For more details, you can refer to my original review  [here]( http://www.slideshare.net/arinto/dremel-reviewrevised "Dremel review").  And these are the slides from Maria:

http://www.slideshare.net/marsty5/googles-dremel
