
### **YARN**

#### **1.集群队列资源卡住，无法分配资源**

解决Patch: Race condition when calling AbstractYarnScheduler.completedContainer. [YARN-3933](https://issues.apache.org/jira/browse/YARN-3933)

```
"AllocateMB":-541696
"AllocateVCores":-249
"AllocateContainers":-252
```

### **HADOOP**

#### **1.NM出现OOM**

解决patch: DefaultMetricsSystem leaks the source name when a source unregisters [HADOOP-13362](https://issues.apache.org/jira/browse/HADOOP-13362)

分析[NodeManager因为ContainerMetric导致OOM](http://hackershell.cn/?p=993)
