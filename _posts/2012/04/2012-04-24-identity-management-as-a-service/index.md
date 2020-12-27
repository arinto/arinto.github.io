---
title: "Identity Management as a Service"
date: "2012-04-24"
categories: 
  - "computing"
  - "school"
tags: 
  - "distributed-computing"
  - "eedc"
  - "identity-management"
---

I read an article titled "[Architecting Cloud Scale Identity Fabric](http://ieeexplore.ieee.org/xpl/login.jsp?tp=&arnumber=5719572&url=http%3A%2F%2Fieeexplore.ieee.org%2Fiel5%2F2%2F5731551%2F05719572.pdf%3Farnumber%3D5719572)" about concept of Identity Management of a Service as part of EEDC assignment. This article is written by Eric Olden, from [Symplified](http://www.symplified.com/). The main thing about this article is about the need of a service to manage user identity in the cloud. Well, I think this diagram from Symplified website is worth more than thousand words:

\[caption id="" align="aligncenter" width="561" caption="Symplified"\]![Symplified's Access Management providing ](images/symplified_access-management.png "Symplified's Identity Management as a Service")\[/caption\]

_Original source of the image:   [http://www.symplified.com/us/products/symplified/overview.html](http://www.symplified.com/us/products/symplified/overview.html)_

As our business grows, we may require multiple solutions from SAAS providers, private clouds or public clouds. However, it will be pretty cumbersome and messy if every service that we use need separate identity that we should manage. It will be more complicated when we factor partners and customers into the equation.

Therefore, in order to ensure scalability and to reduce the complexity (and cost as well to manage the identity), we need a platform or service that can provide us with features likes single-sign-on and cloud-based identity management.

There are some visions that Olden writes in the article related Identity Management as well as its challenge in current cloud computing environment. Without further ado, please refer to our slide below for further details :

http://www.slideshare.net/arinto/architecting-a-cloudscale-identity-fabric
