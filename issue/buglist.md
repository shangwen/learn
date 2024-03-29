
### **YARN**

#### **1.集群队列资源卡住，无法分配资源**

解决Patch: Race condition when calling AbstractYarnScheduler.completedContainer. [YARN-3933](https://issues.apache.org/jira/browse/YARN-3933)

```
"AllocateMB":-541696
"AllocateVCores":-249
"AllocateContainers":-252
```

#### **2.ResourceManager crash due to scheduling opportunity overflow**

解决Path:[YARN-4546](https://issues.apache.org/jira/browse/YARN-4546)
```
[2016-09-01T14:21:01.562+08:00] [FATAL] server.resourcemanager.ResourceManager.run(ResourceManager.java 689) [ResourceManager Event Processor] : Error in handling event type NODE_UPDATE to the scheduler
java.lang.IllegalArgumentException: count cannot be negative: -2147483648
        at com.google.common.base.Preconditions.checkArgument(Preconditions.java:115)
        at com.google.common.collect.Multisets.checkNonnegative(Multisets.java:943)
```
#### **3.AsyncDispatcher may overloaded with RMAppNodeUpdateEvent when Node is connected/disconnected**

解决Yarn上产生的多余事件，导致事件堆积 [YARN-3390](https://issues.apache.org/jira/browse/YARN-3990)

#### **4.FairShareComparator getResourceUsage poor performance**

提升性能[YARN-5969](https://issues.apache.org/jira/browse/YARN-5969)

### **5.YARN RM should avoid unnecessary resolving IP when NMs doing heartbeat**

[YARN-4024](https://issues.apache.org/jira/browse/YARN-4024)

### **6.NodeManager still reports negative running containers**

[YARN-4408](https://issues.apache.org/jira/browse/YARN-4408)

### **7.Resource manager fails with Null pointer exception**

[YARN-4347](https://issues.apache.org/jira/browse/YARN-4347)
```
[2017-06-20T12:33:59.315+08:00] [ERROR] server.resourcemanager.ResourceManager.serviceStart(ResourceManager.java:579) [main-EventThread] : Failed to load/recover state
java.lang.NullPointerException
    at org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler.addApplicationAttempt(FairScheduler.java:688)
    at org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler.handle(FairScheduler.java:1383)
    at org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler.handle(FairScheduler.java:123)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.attempt.RMAppAttemptImpl$AttemptRecoveredTransition.transition(RMAppAttemptImpl.java:1073)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.attempt.RMAppAttemptImpl$AttemptRecoveredTransition.transition(RMAppAttemptImpl.java:1037)
    at org.apache.hadoop.yarn.state.StateMachineFactory$MultipleInternalArc.doTransition(StateMachineFactory.java:385)
    at org.apache.hadoop.yarn.state.StateMachineFactory.doTransition(StateMachineFactory.java:302)
    at org.apache.hadoop.yarn.state.StateMachineFactory.access$300(StateMachineFactory.java:46)
    at org.apache.hadoop.yarn.state.StateMachineFactory$InternalStateMachine.doTransition(StateMachineFactory.java:448)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.attempt.RMAppAttemptImpl.handle(RMAppAttemptImpl.java:791)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.attempt.RMAppAttemptImpl.handle(RMAppAttemptImpl.java:104)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.RMAppImpl.recoverAppAttempts(RMAppImpl.java:836)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.RMAppImpl.access$1900(RMAppImpl.java:101)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.RMAppImpl$RMAppRecoveredTransition.transition(RMAppImpl.java:851)
    at org.apache.hadoop.yarn.server.resourcemanager.rmapp.RMAppImpl$RMAppRecoveredTransition.transition(RMAppImpl.java:841)
    at org.apache.hadoop.yarn.state.StateMachineFactory$MultipleInternalArc.doTransition(StateMachineFactory.java:385)
```

### **8.Yarn recover functionality causes the cluster running slowly and the cluster usage rate is far below 100**

[YARN-4398](https://issues.apache.org/jira/browse/YARN-4398)



### **HADOOP**

#### **1.NM出现OOM**

解决patch: DefaultMetricsSystem leaks the source name when a source unregisters [HADOOP-13362](https://issues.apache.org/jira/browse/HADOOP-13362)

分析[NodeManager因为ContainerMetric导致OOM](http://hackershell.cn/?p=993)

### **HDFS**

#### **1.Scheduling suspect block exiting because of exception**

解决patch:[HDFS-8850](https://issues.apache.org/jira/browse/HDFS-8850)
```
[2016-08-25T11:02:04.067+08:00] [ERROR] server.datanode.VolumeScanner.run(VolumeScanner.java 626) [VolumeScannerThread(/data2/dfs)] : VolumeScanner(/data2/dfs, DS-02fa35a3-b4ab-4727-ab69-c7670f9cf80f) exiting because of exception
java.lang.NullPointerException
        at org.apache.hadoop.hdfs.server.datanode.VolumeScanner.runLoop(VolumeScanner.java:539)
        at org.apache.hadoop.hdfs.server.datanode.VolumeScanner.run(VolumeScanner.java:619)
[2016-08-25T11:02:04.070+08:00] [INFO] server.datanode.VolumeScanner.run(VolumeScanner.java 628) [VolumeScannerThread(/data2/dfs)] : VolumeScanner(/data2/dfs, DS-02fa35a3-b4ab-4727-ab69-c7670f9cf80f) exiting.
```

#### **2.Namenode crashes after Journalnode re-installation in an HA cluster due to missing paxos directory**

解决patch:[HDFS-10659](https://issues.apache.org/jira/browse/HDFS-10659)
```
org.apache.hadoop.hdfs.qjournal.client.QuorumException: Got too many exceptions to achieve quorum size 3/5. 4 exceptions thrown:
xxx.xxx.xxx.xxx:8485: /data0/journal/data/pallas/current/paxos/3544432839.tmp (No such file or directory)
at java.io.FileOutputStream.open(Native Method)
at java.io.FileOutputStream.<init>(FileOutputStream.java:194)
at java.io.FileOutputStream.<init>(FileOutputStream.java:145)
at org.apache.hadoop.hdfs.util.AtomicFileOutputStream.<init>(AtomicFileOutputStream.java:56)
at org.apache.hadoop.hdfs.qjournal.server.Journal.persistPaxosData(Journal.java:964)
at org.apache.hadoop.hdfs.qjournal.server.Journal.acceptRecovery(Journal.java:839)
at org.apache.hadoop.hdfs.qjournal.server.JournalNodeRpcServer.acceptRecovery(JournalNodeRpcServer.java:200)
at org.apache.hadoop.hdfs.qjournal.protocolPB.QJournalProtocolServerSideTranslatorPB.acceptRecovery(QJournalProtocolServerSideTranslatorPB.java:229)
at o
```
采取备份原来的JN的editlog日志，nn日志之后，`namenode -initializeSharedEdits`，把备份的日志覆盖回来即可

#### **3.DFSClient deadlock when close file and failed to renew lease**

[HDFS-9294](https://issues.apache.org/jira/browse/HDFS-9294)

```
Found one Java-level deadlock:
=============================
"Thread-28846":
  waiting to lock monitor 0x00007fa091b06678 (object 0x00000000c0439eb0, a java.lang.Object),
  which is held by "eventHandlingThread"
"eventHandlingThread":
  waiting to lock monitor 0x00007fa090dab208 (object 0x00000000c037c2e0, a org.apache.hadoop.hdfs.LeaseRenewer),
  which is held by "LeaseRenewer:recsys@ns10:8020"
"LeaseRenewer:recsys@ns10:8020":
  waiting to lock monitor 0x000000000210d8a8 (object 0x00000000d5081c40, a org.apache.hadoop.hdfs.DFSOutputStream),
  which is held by "eventHandlingThread"
```

#### **Change "DFSInputStream has been closed already" message to debug log level**

[HDFS-8099](https://issues.apache.org/jira/browse/HDFS-8099)

### **Linux**

#### **1.bad interpreter: Text file busy**

原因：｀The Linux kernel will generate a bad interpreter: Text file busy error if your Perl script (or any other kind of script) is open for writing when you try to execute it.｀
```
NFO Exception from container-launch:
2016-11-11 02:08:42 INFO org.apache.hadoop.util.Shell$ExitCodeException: /bin/bash: /data7/yarn1/local/usercache/dd_edw/appcache/application_1477649572165_814985/container_1477649572165_814985_01_001323/launch_container.sh: /bin/bash: bad interpreter: Text file busy
```
脚本重现
```
#!/usr/bin/env python3
import os
import stat
import subprocess

file = open('./your-script', 'w') # specify full path
try:
    file.write("#!/usr/bin/perl\nprint 'unreachable';") 
    file.flush() # make sure the content is sent to OS
    os.chmod(file.name, 0o555) # make executable
    subprocess.call(file.name) # run it
except Exception as e:
    print(e)
finally:
    os.remove(file.name)
```
