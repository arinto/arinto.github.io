---
title: "Outdated sbt Tutorials at Twitter's Scala School"
date: "2013-10-13"
categories: 
  - "computing"
  - "scala"
tags: 
  - "sbt"
  - "sbt-add-dependency"
  - "sbt-add-task"
  - "sbt-publishing"
  - "scala"
  - "tips"
---

I am currently learning Scala through Twitter's [Scala school](http://twitter.github.io/scala_school "Scala School"), and I found that "Adding Dependencies", "Packaging and Publishing" and "Adding Tasks" tutorials at [Simple Build Tool](twitter.github.io/scala_school/sbt.html) section are outdated. And in this short tutorial, I'll share how I perform the outdated tutorials using **sbt 0.13.0**.

\[caption id="attachment\_1043" align="aligncenter" width="134"\][![This is Scala logo..](images/smooth-spiral.png)](http://www.otnira.com/wp-content/uploads/2013/10/smooth-spiral.png) The logo of Scala\[/caption\]

#### Adding Dependencies

Instead of creating a `.scala` file in `project/build` path, we can easily use the `build.sbt` file to add the required dependencies. Here is the snippet that should be added to `build.sbt`:

`libraryDependencies ++= Seq( "org.scala-tools.testing" % "specs_2.10" % "1.6.+" % "test", "org.codehaus.jackson" % "jackson-core-asl" % "1.9.+" )`

#### Packaging and Publishing

To publish into your local maven repository, instead of using Twitter's `standard-project` as shown in the tutorial, use this following code snippet in your `build.sbt` file:

`publishTo := Some(Resolver.file("file", new File(Path.userHome.absolutePath+"/.m2/repository")))`

Then follow the next step which is to run publish action in your sbt (i.e. type `sbt` in your project directory to load your configuration, and then type `publish`).

For further information about publishing in sbt, refer to this documentation: [http://www.scala-sbt.org/release/docs/Detailed-Topics/Publishing.html](http://www.scala-sbt.org/release/docs/Detailed-Topics/Publishing.html "Publishing")

#### Adding Tasks

Use taskKey to define a task key and use it in your task as shown below (don't forget to put a blank line between time!):

`lazy val print = taskKey[Unit]("Print test message")`

`print := streams.value.log.info("a test message")`

Then, to test it, type `print` in your sbt. The output from your sbt should be something like this:

`> print [info] a test action [success] Total time: 0 s, completed Oct 13, 2013 6:54:18 PM`

Reference for adding new tasks: [http://www.scala-sbt.org/0.13.0/docs/Getting-Started/Custom-Settings.html](http://www.scala-sbt.org/0.13.0/docs/Getting-Started/Custom-Settings.html "Add new tasks")

Ping [me](twitter.com/arinto "arinto") if you know more awesome ways to perform the outdated tutorials! Ciaoooo!!
