---
title: "Java 8 at a glance"
date: "2014-04-07"
categories: 
  - "computing"
  - "talk"
tags: 
  - "compact-profile"
  - "interface-evolution"
  - "java-8"
  - "lambda"
  - "stream"
---

I've just attended a [talk](http://nushackers.org/2014/03/friday-hacks-67-march-21/ "Friday hack") about Java 8 by Chuk Lee at NUS Hackers meetup, and here's the summary. If you're impatient and want to try the code,  go directly to [java-8-at-a-glance repository](https://github.com/arinto/java-8-at-a-glance "java 8 at glance repo"). :)

Chuk started the presentation with short history of JDK, some points to note:

- Oracle acquired Sun on 2010, while JDK  7 was still under development
- At that time JDK 7 was very ambitious project, trying to make JDK more modular (i.e. we can choose which component of JDK to run our app), add lambda expressions and many more.
- Oracle asked Java community to vote, whether to release JDK 7 ASAP with incomplete intended features, or wait longer but with more complete features.
- The Java community chose the first option, and JDK 7 was released on 2011 (5 years after JDK 6 released) with decent [features\[1\]](http://www.oracle.com/technetwork/java/javase/jdk7-relnotes-418459.html "JDK7 rel notes") such as NIO2, type inference in generics, and  fork/join framework.
- However the promised modularity and lambda expression are not available in JDK 7.
- And here they are.. Java 8!

So, what are the highlights in Java 8?

- On language point of view: lambda expression, interface evolution
- On libraries point of view: stream & bulk data operations on collections
- On platform point of view: profiles option, a step towards modularity

\[caption id="attachment\_1134" align="aligncenter" width="400"\][![Nope, we are not talking about Java Island](images/java-island-map.gif)](http://www.otnira.com/wp-content/uploads/2014/04/java-island-map.gif) Nope, we are not talking about Java Island\[9\]\[/caption\]

## Lambda expression

Why do we want lambda expression in Java? Is it because it is cool or because other programming languages (such as Scala and C#) have it? :D

Well, actually the answers are:

- To leverage multicore environment
- Make parallel code easier to use and maintain

But.. why are those the answers? It seems that lambda expression is not related to multicore and parallel code.

The reason is lambda expression allows a piece of code to be stateless and immutable, hence easier to be parallelized and used in multicore environment. Let's see an example of finding the maximum grade from class of 2014:

https://gist.github.com/arinto/9994221

The code snippet above is not stateless (because we need to have temporary variable to hold the current max value and the smartest student) and the collection is mutable (since we can change the value of the collection while we're iterating over it). The result is, we only can run this code in sequential manner and it's not easy to make it parallel.

Using lambda, the code above can be written as

https://gist.github.com/arinto/9994604

And as shown above, the code is stateless (no temporary variable) as well as immutable, hence the VM can easily parallelize it using the available cores.

The lambda expression also introduces closure which is similar to function pointer, but not the same!!  Closure is "a function together with a referencing environment for the non local (free variable) variables of the function. Once defined, the free variables are bound to the function"[\[2\]](http://www.slideshare.net/chukmunnlee/nus-hackers-club-mar-21-whats-new-in-javase-8 "Slides"). The description is a bit superfluous, see this [discussion\[3\]](http://stackoverflow.com/questions/208835/function-pointers-closures-and-lambda "SO-closure") for more information.

Lambda expressions in Java are anonymous functions. They have a typed argument list, a return type, a set of thrown exceptions and a body. Their type is Single Abstract Method i.e. analogous with an interface with only one method such as Runnable. This type is annotated with @FunctionalInterface, but the annotation is optional i.e. similar to @Override.

Another aspect of Java's lambda expression is its type inference (which is formally called [target typing\[4\]](http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html#target-typing "target typing")). This gist below shows examples of target typing.

https://gist.github.com/arinto/10023649

As mentioned in the  gist, the target typing in Java 8 simplifies expression in line 10 into line 12. Target typing is also able to infer the type of variable in the lambda expression as shown in line 22 and 30.

And lastly, there is something called method reference (::) to reuse a method as a lambda expression. Check the sample below for more details:

https://gist.github.com/arinto/10023804

## Interface Evolution

Java 8 has default methods to evolve interfaces without breaking their existing concrete implementations. Basically you can add new methods in existing interfaces with their default implementations. This feature makes interface a bit look like abstract method, but they are not the same. Interfaces do not have internal states and constructors while abstract classes have both of them. Check this example below for more details.

Let's say we have an Awesome interface and this interface already has some implementors, such as AwesomeSpaniard as shown below. The BasicMain class invokes sayHello for each class and the output is shown in BasicMain.out.

https://gist.github.com/arinto/9994695

Prior to Java 8, we need to rewrite all the implementors when we add more functionalities to this interface. But now, the existing concrete implementations may choose to override the new functionalities or use the default implementations. Here is the Awesome interface with its new default method, AwesomeSingaporean choose to override the new method eat and the other classes use the default method.

https://gist.github.com/arinto/9994930

Existing implementations and clients of Awesome interface will still work and do not need to be recompiled. Refer to [this tutorial\[5\]](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html "Default methods") for more examples and details about the default methods. 

## Stream

Notable addition on Java 8 library is java.util. Stream. Stream is a sequence of elements supporting sequential or parallel aggregate operations[\[7\]](http://download.java.net/jdk8/docs/api/java/util/stream/Stream.html "Stream API"). It is pipe/conduit for data flow and it's temporary, thread-safe, stateless and immutable w.r.t the stream source i.e. the collection that is used as stream source will not mutate as a result of the stream operation. And as we see before, immutable and stateless make the Stream operations easy to be parallelize.

Let's say we reuse our student collection in the examples for lambda expressions. Now we want to print all the students graduated on 2014 with grades greater than 90. Using stream, here's the code:

https://gist.github.com/arinto/9995488

Now to parallelize that code, we can use "parallelStream" keyword as shown below:

https://gist.github.com/arinto/9995580

## Compact Profiles

What do you think about the size and contents of JDK? Well, it is big and monolithic, i.e. we need to load javax.swing.JFrame even if we only use classes from java.lang  package. These characteristics cause slow startup time in Java and high memory footprint i.e. minimum memory requirements for JDK 7 application is 128 MB on almost modern Windows OS.

To alleviate the monolithic nature of JDK i.e. make it more modular, compact profiles are introduced. This feature allow us to use subset of JDK, currently there are three types of profiles: compact1, compact2, and compact3. For more details, check this article about [profiles\[6\]](https://blogs.oracle.com/jtc/entry/a_first_look_at_compact "java compact").

Here are some uses cases related to the profile features

### Restricting target compilation

i.e. we can specify the target compilation of our code.

$ javac -profile compact1 Hello.java

### Check compact dependency

We can check the minimum required profile for our compiled classes, using jdeps command.

$ jdeps -P TargetTyping.class

https://gist.github.com/arinto/f5ec2b55c4b486b0a380

[Java documentation site\[7\]](http://docs.oracle.com/javase/8/docs/api/ "Oracle java 8 documentation") has been updated to include the profile information as shown below. In this example, it shows that Remote interface of Java RMI exists in compact2 and compact3 profile.

\[caption id="attachment\_1123" align="aligncenter" width="300"\][![Remote interface of Java RMI](images/Screenshot-2014-04-08-01.09.52-300x175.png)](http://www.otnira.com/wp-content/uploads/2014/04/Screenshot-2014-04-08-01.09.52.png) Remote interface of Java RMI documentation\[/caption\]

So, what's the use for this profile thingy? It enables Java to run on embedded devices and OSes such as Raspberry PI. \*ps: check [embedded Java\[8\]](http://www.oracle.com/technetwork/java/embedded/overview/getstarted/index.html "embedded java") for those who are interested on embedded systems.

## Wrap up!

That's all for this post. Feel free to give comments below or by mentioning me [@arinto](https://twitter.com/arinto "Arinto's twitter"). The code for this post can be found on [Github](https://github.com/arinto/java-8-at-a-glance "Java 8 at a glance").

### References

1. http://www.oracle.com/technetwork/java/javase/jdk7-relnotes-418459.html
2. http://www.slideshare.net/chukmunnlee/nus-hackers-club-mar-21-whats-new-in-javase-8/
3. http://stackoverflow.com/questions/208835/function-pointers-closures-and-lambda/
4. http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html#target-typing
5. http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html
6. https://blogs.oracle.com/jtc/entry/a\_first\_look\_at\_compact
7. http://docs.oracle.com/javase/8/docs/api/
8. http://www.oracle.com/technetwork/java/embedded/overview/getstarted/index.html
9. [http://boncherry.com/blog/2009/09/02/massive-earthquake-hits-java-island/](http://boncherry.com/blog/2009/09/02/massive-earthquake-hits-java-island/)
