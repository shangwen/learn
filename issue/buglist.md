
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
