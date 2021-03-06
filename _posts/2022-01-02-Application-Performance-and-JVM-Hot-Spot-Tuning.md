---
layout: post
title: Application Performance & JVM Hot Spot Tuning
---

| ---- | :----------------------------------------------------------- |
|      |                                                              |
|      | Aspects of Slow Performing Applications                      |
|      | Application Level Problems                                   |
|      | Multi-Threading/Deadlock Issues                              |
|      | Transactional/Synchronizations Issues                        |
|      | Memory Allocation/Management Issues                          |
|      | Bad Application Architecture                                 |
|      | Un-Suitable OR Less Capable Framework Selection              |
|      | Under or Over Estimated Application Capacity or Load         |
|      | Bad Coding Issues                                            |
|      | No Better or Suitable Use of Cache, No SQL DB                |
|      | Bad Query Writing (If Applicable)                            |
|      | Infrastructural Level Problems                               |
|      | Application Layers/Tiers Bottlenecks                         |
|      | IO Calls/Resources Bottlenecks (Application, Cache, DB)      |
|      | Underperformed or Already Heavily Loaded Physical Servers (Application, Cache, DB) |
|      | Not Properly Tuned Physical Servers for OS, DB, Cache, JVM/Tomcat |
|      | Infrastructural Bottlenecks (Firewalls, VPNs, Security Layers, Network Channel) |
|      | Mis-Calculated Failover or Scalability Strategy              |
|      |                                                              |
|      |                                                              |
|      | Aspects To Consider For Great Performing Applications        |
|      | Application Level Considerations                             |
|      | Concurrency Aspects [To Be Taken Care, During Design & Implementation]: Following are important aspects of a distributed, multi-tier, concurrent & multi- threaded applications, which needed to be taken care care of during design & implementation: |
|      |                                                              |
|      | Smooth & Concurrent Application Logging                      |
|      |                                                              |
|      | Graceful Exception/Error Handling especially Deadlocks       |
|      |                                                              |
|      | Graceful Transaction Handling considering ACID Approach (Either Optimistic or Pessimistic Locking) |
|      |                                                              |
|      | Parallel & Sequential Request Handling Capability            |
|      |                                                              |
|      | Usage of Thread Safe & Concurrent Data Structures & Collections |
|      |                                                              |
|      | Adapting & Utilizing Design Patterns e.g. GoF or SOLID       |
|      |                                                              |
|      | Using Code Review Tools e.g. Sonar, FindBug, PMD             |
|      |                                                              |
|      | Adapting & Utilizing Performance Best Practices During Design & Coding |
|      |                                                              |
|      | Code Reviews w.r.t Performance                               |
|      |                                                              |
|      | Regression / Load / Performance Testing                      |
|      |                                                              |
|      | Application Performance Management & Review w.r.t Load & Volume using Regression Testing |
|      |                                                              |
|      | Application Performance Benchmarking using Performance Testing |
|      |                                                              |
|      | Identification of Resources OR Memory Leaking using Profiling Tools |
|      |                                                              |
|      | Optimise Use of Cache & DB                                   |
|      |                                                              |
|      | DBMS Tuning (Proper Clustered & Non-Clustered Indexes & Choice of Rights Indexes w.r.t Load and Data Type) |
|      |                                                              |
|      | Optimized SQL Query Writing (If Applicable), Reviewed Query Execution Plans w.r.t Indexes |
|      |                                                              |
|      | Use of Cache OR No SQL DB for Static or Long Lived Data Objects |
|      |                                                              |
|      |                                                              |
|      |                                                              |
|      | Infrastructural Level Considerations                         |
|      | Scalability Strategy:                                        |
|      | Horizontal (Application Nodes)                               |
|      |                                                              |
|      | Vertical (Application Tiers/Layers)                          |
|      |                                                              |
|      | Failover & High Availability Strategy:                       |
|      | Application Level (Parallel Nodes)                           |
|      | Application Server Level (e.g. Tomcat)                       |
|      | Server Level (e.g. Physical Machine)                         |
|      | Data Center Level (e.g. Data Center A)                       |
|      | Region Level e.g. (e.g. EU, US, LAC, AP, CEMEA)              |
|      | Data Management                                              |
|      | Data Volume Forecasting w.r.t DB Nodes & Disk Space Required (Monthly, Quarterly and Yearly) |
|      | Data Capacity Planning w.r.t Data Volume                     |
|      | Data Archiving & Purging Strategy                            |
|      | DB Load Management, Database Failover & Recovery Strategy    |
|      | Multiple DB Nodes Handling Strategy & e.g. Primary, Secondary, Tertiary |
|      | Syncing & Real-time Backups of DB                            |
|      | Offline Database Backups Strategy                            |
|      |                                                              |
|      | Non-Functional Considerations                                |
|      | Handling N Concurrent Requests/Users                         |
|      | Handling High Data Volume                                    |
|      | Optimization and Performance                                 |
|      | Multi-threading and Concurrency                              |
|      | Portability                                                  |
|      | High Availability                                            |
|      | Vertical & Horizontal Scalability                            |
|      | Security Benchmarking                                        |
|      | Performance Benchmarking                                     |
|      | Threat Modeling & Vulnerability Scans                        |
|      | Continuous Integration                                       |
|      | Multi Hardware, OS, JDK, DB, App Servers, Internet Browsers, Protocols & Use Agents Support |
|      |                                                              |
|      |                                                              |
|      | Fine Tuned JVM for Better Garbage Collection and Application Performance |
|      | The Java Garbage Collector is referred to as a Generational Garbage Collector. Objects in an application live for varying lengths of time depending on where they are created and how they are used. The key insight here is that using different garbage collection strategies for short lived and long lived objects allows the GC to be optimised specifically for each case. Loosely speaking as objects "survive" repeated garbage collections in the Young Generation they are migrated to the Tenured Generation. The Permanent Generation is a special case, it contains objects that are needed by the JVM that are not necessarily represented in your program, for example objects that represent classes and methods. |
|      |                                                              |
|      | Java Heap Memory is part of memory allocated to JVM by Operating System. Whenever we create objects they are created inside heap in java. Java Heap space is divided into three regions or generation for sake of garbage collection called Young Generation, Old or Tenured Generation and Permanent Generation. |
|      |                                                              |
|      | The Young Generation is where all new objects are allocated and aged. When the young generation fills up, this causes a minor garbage collection. A young generation full of dead objects is collected very quickly. Some surviving objects are aged and eventually move to the old generation. |
|      |                                                              |
|      | The Old Generation is used to store long surviving objects. Typically, a threshold is set for young generation object and when that age is met, the object gets moved to the old generation. Eventually the old generation needs to be collected. This event is called a major garbage collection. |
|      |                                                              |
|      | The Permanent Generation contains metadata required by the JVM to describe the classes and methods used in the application. The permanent generation is populated by the JVM at runtime based on classes in use by the application. PermGen has been replaced with Metaspace since Java 8 release. PermSize & MaxPermSize parameters will be ignored now. |
|      |                                                              |

|      | Following are the factors which can affect the performance of JVM and in result performance of application: |
|      |                                                              |
|      | Number of Iterations of Full/Major Garbage Collection        |
|      | Uncontrolled & Frequent Iterations of Minor Garbage Collection |
|      | Managing Heap Size and Stack Size / Thread Process Space     |
|      | JIT Optimization                                             |
|      | Selection of Appropriate GCs For Young & Tenured Memory Space |
|      | Running JVM On Defaults w/o Understanding Nature of System   |
|      | Scope of Tuning of System e.g. For Throughtput, Handling Short or Long Lived Objects, Batch Routines, Heavy Processing |
|      | Non-Heap GC                                                  |
|      | Although applications performance is mainly rely on, how it is developed and/or aptitude of programming and/or good programming practices. Surely tuning JVM will not solve problems related to bad programming or selection of inappropriate data structures. But with the help fine tuned JVM we can get better results w.r.t handling of unusual load and better throughput and memory management. |
|      |                                                              |
|      | Proposed JVM Settings                                        |
|      |                                                              |
|      | -server -Xss4m -Xms4096m -Xmx4096m -XX:+UseG1GC -XX:+HeapDumpOnOutOfMemoryError -XX:+AggressiveOpts -XX:+DoEscapeAnalysis -Xnoclassgc -XX:+UseBiasedLocking -XX:ReservedCodeCacheSize=48m -XX:+UseCompressedOops |
|      | -XX:+UseStringDeduplication -XX:MaxGCPauseMillis=200 -XX:GCPauseIntervalMillis=4000 |
|      |                                                              |
|      | -XX:+AggressiveOpts: JVM performance optimisation, it enables some internal mechanism of JVM for better throughput and memory management |
|      |                                                              |
|      | -XX:+UseBiasedLocking: Performance optimisation of locking mechanism specifically synchronization |
|      |                                                              |
|      | -XX:+DoEscapeAnalysis: less GC activity and 14x faster execution of code |
|      |                                                              |
|      | -XX:ReservedCodeCacheSize: JIT optimization                  |
|      |                                                              |
|      | -XX:+UseStringDeduplication: https://blog.codecentric.de/en/2014/08/string-deduplication-new-feature-java-8-update-20-2/ |
|      |                                                              |
|      | -XX:+UseCompressedOops: When this option is enabled, object references are represented as 32-bit offsets instead of 64-bit pointers, which typically increases performance when running the application with Java heap sizes less than 32 GB. This option works only for 64-bit JVMs. |
|      |                                                              |
|      | As we are aware that all the GCs are "Stop The World", even CMS and G1GC when there is need of Full/Major GC. But G1GC has special capability, that it'll collect memory with minor garbage collection during normal transaction processing without being stoping any operations/thread within JVM. Because G1Gc supports Parallelism, Concurrency and Multi-Threading. |
|      |                                                              |
|      | The purpose of these below JVM Params controlling Major GC:  |
|      |                                                              |
|      | -XX:MaxGCPauseMillis=200: The value of this param will pause/stop all threads for 200 Millis for Garbage Collection. |
|      |                                                              |
|      | -XX:GCPauseIntervalMillis=4000: Using value of this params G1GC will look after 4 seconds to see if there is need of garbage collection or not. |
|      |                                                              |
|      | Resources                                                    |
|      | Kindly have a look at these important resources for better understanding and getting idea about Garbage Collection. |
|      |                                                              |
|      | http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html |
|      |                                                              |
|      | http://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html |
|      |                                                              |
|      | http://www.oracle.com/technetwork/tutorials/tutorials-1876574.html |
|      |                                                              |
|      | http://javaproseeker.blogspot.jp/2014/08/anatomy-of-g1-garbage-first-collector.html |
