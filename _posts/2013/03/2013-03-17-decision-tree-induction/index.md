---
title: "Decision Tree Induction"
date: "2013-03-17"
categories: 
  - "projects"
  - "school"
  - "thesis"
tags: 
  - "decision-tree"
  - "divide-and-conquer"
  - "machine-learning"
  - "thesis"
---

After [learning some basics](http://www.otnira.com/2013/03/10/bootstrapping-machine-learning/ "Bootstrapping Machine Learning") about Machine Learning (ML), time to get into the details related to my thesis. After discussing with my supervisors, we decided to implement classification algorithm based on **decision tree**. So, in this post, I would like to give an overview about decision-tree in ML.

\[caption id="" align="aligncenter" width="533"\][![An example of decision tree from XKCD](images/flow_charts.png)](https://xkcd.com/518/) An example of decision tree from XKCD ;)\[/caption\]

### **What is decision-tree?**

Decision-tree is the common output of a divide-and-conquer approach in learning from a set of independent instances. A decision tree consists of nodes and branches. Each node consists of questions based on one or several attributes i.e. compares an attribute value with a constant or it could compare more than one attributes using some functions. Learning data set to produce a decision tree is often called tree-induction.

Figure below shows example of the original data-set and its associated decision tree:

 

\[caption id="attachment\_760" align="aligncenter" width="586"\][![Labor wage negotiations data](images/Wage-data-cleaned.png "Labor wage negotiations data")](http://www.otnira.com/wp-content/uploads/2013/03/Wage-data-cleaned.png) Labour wage negotiations data\[/caption\]

 

[![Labor wage negotations decision tree](images/Wage-decision-tree.rotated.png)](http://www.otnira.com/wp-content/uploads/2013/03/Wage-decision-tree.rotated.png)

Labour wage negotiations decision tree

 

The labour negotiations training data-set can be transformed into different decision trees and it depends on the actual implementation of the tree induction. Figure 1.3 shows two types of trees, the tree on the right-side has more nodes and it seems that it fits the existing training data-set well. The tree on the left-side is the pruned version of the right-side-tree. So now, which one is better decision tree?

One is tempted to choose the right-hand-side because it really fits the existing data-set well that used to build the tree. But, be careful, in machine learning, what we care is the accuracy in predicting the class of new dataset. The right-hand-side will have high accuracy when we use it to predict the class of the training data-set, but it may have more difficulties in predicting new data-set that presented to our decision tree. This problem is commonly referred as over-fitting i.e. the decision tree is over-fitted to the training data-set.

To avoid this over-fitting problem, decision tree is often pruned based on some criterion. And the one of the pruned decision tree is shown on the left-hand-side. But, again, be careful not to prune too much nodes. We also do not want our tree to be so generic that it under-fit our dataset.

In conclusion, our tree should not be too generic and too over-fit in relation with our training dataset.

### **Divide-and-conquer Approach in Building Decision Tree**

Let’s say we have this following weather dataset and we want to build a decision tree using divide-and-conquer approach. Our classification goal is to decide whether it’s time to play or not to play based on available weather attributes data.

\[caption id="attachment\_755" align="aligncenter" width="579"\][![Weather dataset](images/Divide-and-conquer-sample-dataset-rotated.png)](http://www.otnira.com/wp-content/uploads/2013/03/Divide-and-conquer-sample-dataset-rotated.png) Weather dataset\[/caption\]

We are going to build it by using divide-and-conquer approach, that means we will start building the tree from the root. At the moment, let’s ignore the ID code attribute first. We need to choose between “Outlook”, “Temperature”, “Humidity” and “Windy” attributes as our attribute to split the tree. The output class is “Play” and it only has two possible values: “yes” and “no”. Our possible split at root is one of these four possibilities. Stump is a tree which only have one node.

\[caption id="attachment\_761" align="aligncenter" width="420"\][![Weather decision stump](images/DecisionStump-cleaned.png)](http://www.otnira.com/wp-content/uploads/2013/03/DecisionStump-cleaned.png) Weather decision stump\[/caption\]

Now, how are we going to decide which attribute to choose? Which is the best choice? Intuitively, the leaf which only have one class, will not have to be splitted further. This kind of leaf is called “pure leaf”. In general, we want to have attributes that splits the dataset with highest purity measure. This purity is measured in term information and its unit is bits. To calculate the information, the most basic method is using [entropy](https://en.wikipedia.org/wiki/Entropy_%28information_theory%29 "Entropy") formula developed by the father of information theory, [Claude E. Shannon](https://en.wikipedia.org/wiki/Claude_E._Shannon "Shannon"). The entropy of a class random variable that takes on c values with probabilities _p1,p2,...,pc_ is given by:

$latex \\sum^{c}\_{i=1}{-p\_ilog\_2p\_i}$

To decide the attribute for splitting, we compare the information gain for each attribute. Information gain can be calculated using this formula:

> information gain = expected information by splitting on specific attribute - information obtained without any splitting

 Let’s take a concrete example, which is “outlook” attribute. Here are the calculation steps:

1. First we need to find out expected information by splitting on specific attribute, in this case out attribute is “outlook” that has three values:
    - "sunny", which results in 2 occurrences of class "yes" and 3 occurrences of class "non". This will results in information: $latex info(\[2,3\]) = -p\_1log\_2p\_1-p\_2log\_2p\_2$ where $latex p\_1$ is the probability of "yes" in sunny outlook (with value of $latex \\frac{2}{2+3} = \\frac{2}{5}$) and $latex p\_2$ (with value of $latex \\frac{3}{2+3} = \\frac{3}{5}$) is the probability of "no" in sunny outlook. Plugging both _p_ values into information formula we obtain $latex info(\[2,3\])= -\\frac{2}{5}log\_\\frac{2}{5}-\\frac{3}{5}log\_2\\frac{3}{5}=0.971 bits$
    - "overcast", which results in 4 occurrences of class "yes". $latex info(\[4,0\]) = 0.0 bits$
    - "rainy", which results in 3 "yes"s and 2 "no"s. $latex info\[(3,2)\]=0.971 bits$
2. Expected information is calculated by: $latex info(\[2,3\],\[4,0\],\[3,2\])=\\frac{5}{14}info(\[2,3\])+\\frac{4}{14}info(\[4,0\])+\\frac{5}{14}info(\[3,2\])=0.693 bits$
3. Now we need to find the amount of information without any splitting $latex info(\[9,5\])=0.940 bits$
4. The information gain is: $latex gain(outlook) = info(\[9,5\])-info(\[2,3\],\[4,0\],\[3,2\])=0.247 bits$

The intuitions behind this are:

1. Without any split, we need additional information of 0.940 bits to determine a particular instance is in class “yes” or “no”.
2. By splitting based on “outlook” attribute, we need additional information of 0.693 bits to determine a particular instance is in class “yes” or “no”.
3. Therefore by splitting on “outlook” attribute, we gain information value of (0.940 - 0.693) = 0.247

After calculating the “outlook” attribute, we need to continue calculate for other attributes:

1. gain(outlook) = 0.247
2. gain(temperature) = 0.029
3. gain(humidity) = 0.152
4. gain(windy) = 0.048

Therefore for the root node, we will choose “outlook” as the attribute for splitting the tree branches. We continue this process recursively until we reach terminating condition of tree growing.

### **The Curious Case of Highly Branching Attribute**

Refer back to the weather dataset that we used in the previous section. We omitted IDcode attributes when performing root calculation. Now, let’s say we include the IDcode when we are choosing the attribute for the root. What is the information gain of IDcode?

Well, let’s take a look at this decision stump of IDcode and the following information calculation:

\[caption id="attachment\_762" align="aligncenter" width="380"\][![Weather decision stump based on IDCode](images/IDcode-stump.png)](http://www.otnira.com/wp-content/uploads/2013/03/IDcode-stump.png) Weather decision stump based on IDCode\[/caption\]

Information of IDcode is $latex info\[(0,1),(0,1),(1,0) .... (1,0),(0,1)\] =0$.

Hence the information gain is $latex info\[(9,5)\] -0=0.940 -0=0.940$

Wow, the information gain is very high, and it is even higher than the gain of “outlook” attribute. But is it what we truly desire? Unfortunately the answer is no. Branching in IDcode is extremely good for training data-set but it is extremely not good for predicting the class of unknown instances and it also contains no knowledge on the decision structure. To compensate this, **gain ratio** is introduced. The gain ratio can be calculated by taking into account the number and size of the daughter nodes, but ignoring any information about the class distribution of the daughter nodes.

In the IDcode example above, the gain ratio is calculated by these following steps:

1. $latex info\[(1,1,1,1....1)\]=-\\frac{1}{14}log\_2\\frac{1}{14}\\times14=3.807$
2. $latex gainratio(IDCode)=\\frac{originalInformationGainOfIDCode}{info\[(1,1,1,1...1)\]}=\\frac{0.940}{3.807}=0.247$

Now let's compare with gain ratio of "outlook"

1. $latex info\[(5,4,5)\]=-\\frac{5}{14}log\_2\\frac{5}{14}-\\frac{4}{14}log\_2\\frac{4}{14}-\\frac{5}{14}log\_2\\frac{5}{14}=1.577$
2. $latex gainratio(outlook)=\\frac{originalInformationGainOfOutlook}{info\[(5,4,5)\]}=\\frac{0.247}{1.577}=0.157$

Continuing the calculation of gain ratio for other attributes, we obtained these following values:

1. gain ratio(IDcode)=0.247
2. gain ratio(outlook)=0.157
3. gain ratio(temperature)=0.019
4. gain ratio(humidity)=0.152
5. gain ratio(windy)=0.049

Based on those values, IDcode still preferred but its advantage is greatly reduced. Humidity now comes very close to outlook because it splits the tree into two branches instead of three branches.

### **Conclusion**

This summary involves the basic information gain and gain ratio calculation. It is not yet ready for practical use. Several improvements should be performed to make this tree induction ready for practical and production use. One of widely used decision tree is called C4.5 developed by J. Ross Quinlan and I'm planning to post my summary on it. I'll put the link once I created the post ;)

\*All sample dataset and diagram are taken from [WEKA book](http://www.cs.waikato.ac.nz/ml/weka/book.html "Weka book").
