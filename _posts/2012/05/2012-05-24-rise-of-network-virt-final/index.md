---
title: "Rise of Network Virtualization - Final"
date: "2012-05-24"
categories: 
  - "computing"
  - "school"
tags: 
  - "distributed-computing"
  - "eedc"
  - "ethane"
  - "network-virtualization"
  - "nicira"
  - "openflow"
---

This is the follow-up-post of [this post](http://www.otnira.com/2012/05/01/riseofnetworkvirtualization/) about EEDC project this semester. Well, I would like to have more time to polish my slides but that's fine. It's already past :)

I used historical perspective to discuss about the "rise" of network virtualization, in this context I focused on SDN (Software-Defined Networking) and it is all started in 2007. At that time, network are faster, but not better. It has several limitations such as high complexity in maintaining it, high possibility of policy inconsistent across devices in the network, inability to scale, and dependency on vendor. On the other hand, the need of new network architecture is crucial due to traffic pattern change (not only from client to server or vice versa, but also between nodes in server cluster), consumerization of IT, and rise of cloud service and Big Data.

Instead of covering Remus, I found more appropriate network virtualization technology from circa 2007 to cover in this project. It is the embryo of Nicira, called [Ethane](http://klamath.stanford.edu/~nickm/papers/ethane-sigcomm07.pdf). It's a PhD project in Stanford and it allows network manager to **define single network-wide fine-grain policies.** It tries to solve über-complexity problem in managing network and make network better. It works based on three following principles:

1. Network policies declared over high-level names
2. Policy determines path
3. Strong binding between a packet and origin

Figure below shows Ethane in a nutshell.

\[caption id="attachment\_300" align="aligncenter" width="507" caption="Communication flow in Ethane"\][![Ethane in a nutshell](images/EthaneComm.png "Communication flow in Ethane")](http://www.otnira.com/wp-content/uploads/2012/05/EthaneComm.png)\[/caption\]

If user A wants to send packets into user B, the packet will arrive at switch 1 and switch 1 will route the packet the Controller. The Controller will determine the route of the packet and forward it into switch 2 where user B connected.

One may argue that the scalability will be limited and network performance will be compromised. However, Ethane researchers claim that Ethane is able to scale well and network performance is still on par with normal network.

In 2008, Ethane inspires [OpenFlow](http://www.openflow.org/) standard. Related publication can be found [here](http://dl.acm.org/citation.cfm?id=1355746). OpenFlow tries to solve the problem of lack of experiment of network protocol due to **limited realistic testbed**. OpenFlow utilizes campus network as a realistic network testbed that allows researches to configure it **easily** to fit their experiment requirements.

OpenFlow has Controller concept of Ethane as shown in figure below

\[caption id="attachment\_303" align="aligncenter" width="300" caption="OpenFlow Switch"\][![OpenFlow Switch](images/OpenFlowSwitch-300x254.png "OpenFlow Switch")](http://www.otnira.com/wp-content/uploads/2012/05/OpenFlowSwitch.png)\[/caption\]

 

Then in March 2012, a company called Nicira, publicly launched their product. Well, the interesting thing is, Nicira is founded in 2007 by sub-group of people who published Ethane :D. And it took them 5 years to create a solid network virtualization product. Nicira's products are **Network Virtualization Platform(NVP)**. It likes the Matrix, but for nodes in network. NVP allows network manager to reprogram the network used by programs and applications. It allows our data center to move like a "liquid". We can take snapshot of our data center network, and migrate the snapshot to our another set of data center. We can roll back the state of our data center network or even replay some of the events that are happening in our network.

NVP **decouples** between physical network resources with application that using it as shown in figure below. Note that the main concept of NVP is similar to desktop virtualization, whereby physical compute capacity is decoupled with OS that running on top of them.

\[caption id="attachment\_304" align="aligncenter" width="300" caption="Nicira Network Virtualization Platform"\][![Nicira Network Virtualization Platform](images/NetworkVirtualizationPlatform-300x294.png "Nicira Network Virtualization Platform")](http://www.otnira.com/wp-content/uploads/2012/05/NetworkVirtualizationPlatform.png)\[/caption\]

To summarize the discussion, I present my conclusion which are

1. Software Defined Networking (SDN) is the trend of network virtualization
2. SDN allows use to perform transition from **managing network complexity** to **abstracting complexity**.
3. Ethane, OpenFlow and Nicira are real example of innovation transition from school/research environment into specification and enterprises

Without further ado, I'll present you the slides. The references can be found on the last two slides. http://www.slideshare.net/arinto/rise-of-network-virtualization
