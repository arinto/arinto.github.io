---
title: "Java, Eclipse, & Maven on Mac OS X Mavericks"
date: "2014-02-08"
categories: 
  - "computing"
  - "tips"
tags: 
  - "eclipse"
  - "java"
  - "mac"
  - "maven"
  - "mavericks"
  - "os-x"
  - "osx"
---

Hola a todos!!!

It has been 4 months since my last post. I have been busy mainly with my job with [LARC](http://larc.smu.edu.sg/ "LARC") (and other extra commitments such as classes at Masjid) and could not allocate time to continue updating this blog :(. But from now, I'll allocate one of my weekends every month to update this blog. :) \*wish me all the best!! :)

I've just received my new MacBook Pro, and just upgraded to OS X 10.9.1. And I found that setting up my usual Java development environment is not as straightforward as in Linux (the steps are similar, but not the same!!). Hence, here I present how I setup my Java development environment.

## JDK

Apple actually has already provided its own JDK (it will prompt you to install it actually), but unfortunately it is still JDK 6. Unless you're content with JDK6, you can easily install JDK7 from Oracle [website](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html "JDK7") and follow the steps in [here](http://docs.oracle.com/javase/7/docs/webnotes/install/mac/mac-jdk.html "JDK7-How-to-install"). In the end of the installation process, you should check the "java" and "javac" version and you should also have Java control panel in "System Preferences".

\[caption id="attachment\_1071" align="aligncenter" width="600"\][![java and javac version](images/Screenshot-2014-02-08-23.50.56.png)](http://www.otnira.com/wp-content/uploads/2014/02/Screenshot-2014-02-08-23.50.56.png) java and javac version\[/caption\]

 

\[caption id="attachment\_1073" align="aligncenter" width="600"\][![System Preferences with Java control panel](images/Screenshot-2014-02-08-23.59.49.png)](http://www.otnira.com/wp-content/uploads/2014/02/Screenshot-2014-02-08-23.59.49.png) System Preferences with Java control panel\[/caption\]

And don't forget to setup the JAVA\_HOME environment variable by creating .bash\_profile if it doesn't exist and put this script \[reference: [article](http://mwebhack.blogspot.sg/2012/12/set-up-java-7-eclipse-and-netbeans-on.html "Set up Java 7 on Mac OSX")\]

export JAVA\_HOME=\`/usr/libexec/java\_home -v 1.7\`

## Eclipse

Well, it is almost straight forward. Download your Eclipse (\*.tar.gz) from its [website](http://www.eclipse.org/downloads/ "Download eclipse"). Extract the tar.gz file into an 'eclipse' folder. Then, just copy or move the 'eclipse' folder into OS X's Applications folder. Make sure you have this folder structure in you Applications folder.

\[caption id="attachment\_1077" align="aligncenter" width="600"\][![Eclipse folder structure in Applications folder](images/Screenshot-2014-02-09-00.25.19.png)](http://www.otnira.com/wp-content/uploads/2014/02/Screenshot-2014-02-09-00.25.19.png) Eclipse folder structure in Applications folder\[/caption\]

Then, you can drag-and-drop the Eclipse launcher into Dock for easy access. Now, click the Eclipse launcher and see what happened. If your Eclipse run properly, than it's done!

But.. if Mavericks prompts you to install Apple JRE and you want to use JRE 7, then you need to do this workaround outlined in this [discussion](http://stackoverflow.com/questions/19563766/eclipse-kepler-for-os-x-mavericks-request-java-se-6/19594116#19594116 "Eclipse on Mavericks"). (don't forget to upvote the answer! :))

## Maven

Mavericks does not include Maven by default. Hence, the easiest way is to use [Homebrew](http://brew.sh/ "homebrew ") to install it. Follow this [instruction](http://stackoverflow.com/questions/3987683/homebrew-install-specific-version-of-formula "Homebrew install older version") if you need to install older version of Maven (Maven2 or Maven 3.0.5).

Voila! You have JDK, Eclipse, and Maven installed in your OSX Mavericks! That's all for now, feel free to give comments and suggestions!
