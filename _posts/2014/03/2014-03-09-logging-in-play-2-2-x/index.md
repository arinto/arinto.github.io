---
title: "Configuring Logging in Play 2.2.x"
date: "2014-03-09"
categories: 
  - "computing"
  - "projects"
tags: 
  - "log"
  - "logback"
  - "logging"
  - "play-2-2-x"
  - "play-framework"
---

Well, actually it is pretty straightforward, just follow this [Play 2.2.x documentation](http://www.playframework.com/documentation/2.2.x/SettingsLogger "Play 2.2.x Logger Settings"). But there is a caveat that costs me several hours to resolve.

\[caption id="attachment\_1090" align="aligncenter" width="300"\][![Play Framework](images/normal-play-300x102.png)](http://www.otnira.com/wp-content/uploads/2014/03/normal-play.png) Play Framework\[/caption\]

Based on Play 2.2.x documentation , we should use \`-Dlogger.resource\` after the command \`start\` inside Play console, i.e

\[OS-console\] $ play
\[info\] Loading project definition from ....
\[info\] ...
..... //initialization message from Play
\[play-console\] $ start -Dlogger.resource=my-logging-configuration.xml

But what if we want to invoke it outside Play console? Does  this command below work properly?

play start -Dlogger.resource=my-logging-configuration.xml

Well the answers is NO!

So, I add some code on my controller to print the logging configuration file (which is [logback](http://logback.qos.ch/ "logback") configuration file) and Play does not consider the VM arguments (i.e. -Dlogger.resource) using above command. Logback  uses the default Play's logger.xml in  \`$PROJECT\_HOME/play-2.2.x/repository/local/com.typesafe.play/play\_2.10/2.2.x/jars/play\_2.10.jar!/logger.xml\`

So, to resolve this, we should invoke the "play start" command as below:

play -Dlogger.resource=my-logging-config.xml -Dother.vmarg=val start

That's all for this month! See you (hopefully earlier than) next month!
