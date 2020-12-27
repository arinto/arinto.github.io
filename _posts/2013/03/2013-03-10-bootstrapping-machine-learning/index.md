---
title: "Bootstrapping Machine Learning"
date: "2013-03-10"
categories: 
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "machine-learning"
  - "thesis"
  - "weka"
---

My thesis will be related to machine learning(ML), therefore, I need to learn the necessary ML knowledge to do the project. In this post, I would like to revisit some concepts and materials that I used to start learning about ML. Feel free to comment and give suggestions!

Machine Learning is not statistics and not data-mining, but it is in between them. ML is more like automated application of statistics to perform data mining tasks i.e. ML develops algorithms for making predictions from data. Note that predictions in this context refers to statistical-prediction.

\[caption id="" align="aligncenter" width="640"\][![](images/176639.strip.gif)](http://dilbert.com/strips/comic/2013-02-02/) Not this kind of Machine Learning though :p\[/caption\]

Data in ML consists of _data instances._ The data instances are represented as _feature vectors_. Example: people can be represented as feature vectors of height and weight, such as:

- Arinto -> (158,55)
- Lionel Messi (169, 70)

We can add more features into our data instances. Features are chosen for a specific task at hand i.e. what do we want to accomplish from the data. The cool term for this concept is _feature engineering._

Machine learning consists of three groups of methodology:

1. Classification. Given a new data instance, we want to infer/predict in which group/class the new data instance belongs to.
2. Clustering. Given a set of data instances, we want to group subsets of them that have similar characteristics.
3. Regression. Give a set of data, we are trying to fit some lines into our existing dataset, so that we can predict some output based on given inputs.

I will not go deep into each of them for this post, but I will list some pointers about learning ML. Here they are:

1. [Machine Learning: The Basics](http://www.youtube.com/watch?v=wjTJVhmu1JM "Machine Learning: The Basics"), by Ron Bekkerman. Very intuitive and clear explanation about the what machine learning is and what are the three groups of methodology.
2. [Coursera's Machine Learning Course](https://class.coursera.org/ml/ "Coursera's Machine Learning course"), by Andrew Ng.
3. [Data Mining: Practical Machine Learning Tools and Techniques](http://www.cs.waikato.ac.nz/ml/weka/book.html "Data mining book") book,. And, don't forget to get your hands dirty with [WEKA](http://www.cs.waikato.ac.nz/ml/weka/ "WEKA") ML framework. FYI, this book is often called _WEKA-book_.

To conclude this post, I would like to quote a tip from a fellow Yahoo! intern, Martin ([@fris](https://twitter.com/fris "Martin's twitter")), who has been in machine learning for several years:

> Machine Learning is huge. Start picking them up by studying at the high level first i.e. don't go into details yet, read all the introduction or first few paragraphs of the all chapters in WEKA-book. Once you're focusing on one part of machine learning, you can go drill down into the required details :)
