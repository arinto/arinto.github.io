---
title: "Wuala - P2P Distributed Storage"
date: "2012-03-25"
categories: 
  - "computing"
tags: 
  - "distributed-computing"
  - "distributed-data-storage"
  - "file-sharing"
  - "p2p"
---

When I was looking for example of P2P distributed storage system, I came across [video](http://www.youtube.com/watch?v=3xKZ4KGkQY8) from Google Tech Talk about [Wuala](http://wuala.com/). **Wuala** is an example of distributed peer-to-peer storage system.  It used to be startup company, but LaCiE bought it in 2009. It allows you to store your data in the cloud, set up online back up and access your files, share your files with your friends easily, and access them from other computer.

![](images/wuala_logo-HORIZ_400x160.png "Wuala")

It puts emphasize on **security** by performing encryption on our data in Wuala client and divide the encrypted data into several chunks and spread them in the network. The interesting thing about Wuala is the **peer-to-peer nature** of this application. The developers of this application need to ensure the availability of files and incentive mechanism for the peer.

In the early stage of this application, users can trade their storage with online and I think it is one of the advantages of Wuala with other distributed storage system such as [Dropbox](https://www.dropbox.com). But now, this feature is no more :( and IMHO, make it not as good as Dropbox.

This video explains what how they design Wuala with these following key points

1. **Availability** is ensured using redundancy by erasure code. FYI, they explain the concept of erasure coding very well. :D
2. To **maintain** the file, each Wuala clients check the file in the network periodically.
3. **Nodes** in Wuala can be classified into three type: super nodes (perform routing), storage nodes(store the data chunk), and client nodes(Wuala client)
4. Their network is **not really structured**, because super nodes connect to their direct neighbor and random links to reach further nodes
5. To prevent free-riding, they introduce **incentive and fairness mechanism** based on size of shared disk and online time.
6. Perform **transaction observation** to determine the upload rate to other peer based on reputation system.
7. Use 128 AES for encryption and 2048 bit RSA
8. Use [Cryptree](http://ieeexplore.ieee.org/xpl/freeabs_all.jsp?arnumber=4032481) to regulate the **access control** to the stored files

In the end of the video, the speaker gives short demo of Wuala and he has Q & A session with Google engineers.

Well, for more details, you can check the video in this [link](http://www.youtube.com/watch?v=3xKZ4KGkQY8)!
