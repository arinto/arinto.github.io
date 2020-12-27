---
title: "Why file sharing network is dangerous?"
date: "2012-03-05"
categories: 
  - "school"
tags: 
  - "distributed-computing"
  - "file-sharing"
  - "p2p"
---

Recently I have just finished paper review assignment about file sharing network. The paper title is _[Why File Sharing Network is Dangerous?](http://dl.acm.org/citation.cfm?id=1461962)_ by M. Eric Johnson, Dan Mcguire and Nicholas D. Wiley. And I'll present you, my summary:

**Introduction**

The paper explains about security aspect of P2P file sharing network in term of confidential data exposure. P2P file sharing network is widely used now, and according to their research, the user base is doubled from 2003 to 2007. (yes it is paper from 2007!.. so not really the latest and greatest paper:)). Its analysis is based on 1st generation P2P file sharing network that are utilized by P2P file application such as KaZaA, Frostwire and eMule. Those application has common characteristics which is user needs to explicitly share the file and folder for downloading and uploading purpose.

**Problem Statement**

How secure are the aforementioned P2P application? What are the possible threat that may exist on them?

**Proposal**

Perform two types of experiments to answer the question in the problem statement.

1. Searching for confidential file in P2P network
2. Honeypot experiment, by exposing several files containing confidential information and observing their distribution.

**Hypotheses**

The paper indirectly shows these following hypotheses:

1. P2P file sharing network and application are relatively dangerous. User may expose their confidential file unintentionally to the file sharing network due to their negligence or malicious P2P client.
2. The threat still exists although user has stopped sharing the file. That means, there is some possibility that the files still present in the file sharing network and attacker may be able to obtain it.

**Experiment Result**

1. Search file experiment: Considerable amount of confidential documents are found, including birth certificates (45 results) , passport (42 results), and tax return (208 results). According to the writer of the paper, the future is not so bright. More leaks, more loses and more malware due to the growing usage of P2P application. However, this result deserves to be further investigated. Someone may argue that 45 results out of millions of peers that are using the P2P network are not so bad.
2. Honeypot experiment: The confidential documents that are shared consists of prepaid credit card number with valid balance of 25 USD and a phone card with some balance. Both cards have no balance after few days. They also observed that the files are distributed until as far as Singapore and Europe. (The origin of the files are in United States).

**Conclusion**

They conclude their researches by suggesting these following counter-measures

1. Improvement of UI design of P2P client application
2. User education to be more careful in using P2P application
3. Introduction of file naming and organization to avoid sharing the confidential file accidentally. It's a bit weird in my opinion, because logically in an enterprises environment, P2P application is strictly banned. So, since the application is banned in enterprises, this suggestion doesn't really make sense.

And I host the slide in Slideshare as usual :

http://www.slideshare.net/arinto/why-file-sharing-is-dangerous
