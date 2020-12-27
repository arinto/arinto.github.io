---
title: "C4.5 Decision Tree Implementation"
date: "2013-03-25"
categories: 
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "c4-5"
  - "decision-tree"
  - "machine-learning"
  - "thesis"
---

It's time to go deeper in decision tree induction. In this post, I'll give summary on real-world implementation (i.e. the implementation has been used in actual data mining scenario) called C4.5.

### **C4.5**

C4.5 is collection of algorithms for performing classifications in machine learning and data mining. It develops the classification model as a decision tree. C4.5 consists of three groups of algorithm: C4.5, C4.5-no-pruning and C4.5-rules. In this summary, we will focus on the basic C4.5 algorithm

#### Algorithm

In a nutshell, C4.5 is implemented recursively with this following sequence

1.     Check if algorithm satisfies termination criteria
2.     Computer information-theoretic criteria for all attributes
3.     Choose best attribute according to the information-theoretic criteria
4.     Create a decision node based on the best attribute in step 3
5.     Induce (i.e. split) the dataset based on newly created decision node in step 4
6.     For all sub-dataset in step 5, call C4.5 algorithm to get a sub-tree (recursive call)
7.     Attach the tree obtained in step 6 to the decision node in step 4
8.     Return tree

For alternative description, check out C4.5 in pseudocode flavor below:

\[caption id="attachment\_790" align="aligncenter" width="502"\][![Pseudocode of C4.5](images/C4.5.png)](http://www.otnira.com/wp-content/uploads/2013/03/C4.5.png) Pseudocode of C4.5\[/caption\]

#### What types of test (i.e. the question in each node) are possible?

Not only binary, but also n-ary branches/outcomes are supported by C4.5.

#### How are test chosen?

Information-theoretic criteria such as gain and gain ratio are used to determine the best test greedily.

#### How are test threshold chosen?

For nominal attribute, it is straightforward i.e. different possible instantiations of that attribute. For numeric attributes, the threshold is chosen by soring the attribute values, and choose a split between successive values that maximize the information-theoretic criteria. Not all splits between successive values need to be considered, it only needs to consider splits between successive different values after they are sorted.

#### How is tree-growing terminated?

When the all instances that covered by a specific branch are pure, OR, the number of instances fall below a certain threshold.

#### How are class labels assigned to the leaves?

Majority class of the instances in a specific leaf is taken to be the class prediction.

#### C4.5 Features

C4.5 has additional features such as tree pruning, improved use of continuous attributes, missing values handling and inducing ruleset. For those who are interested, you can read the source of this summary, which is Chapter 1 of [Top Ten Algorithms in Data Mining](http://www.amazon.com/Algorithms-Mining-Chapman-Knowledge-Discovery/dp/1420089641 "Top ten algo").

\*Pseudocode is obtained from Top Ten Algorithms in Data Mining [book](http://www.amazon.com/Algorithms-Mining-Chapman-Knowledge-Discovery/dp/1420089641 "Top ten algo").
