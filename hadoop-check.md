
## NOTES:
### 1. You can check the status of HDFS by running this command:
```
[root@10 svc]# kubectl -n test-test exec -it hdfs-nn-0 -- /usr/local/hadoop/bin/hdfs dfsadmin -report
Configured Capacity: 18238930944 (16.99 GB)
Present Capacity: 16152424448 (15.04 GB)
DFS Remaining: 16152420352 (15.04 GB)
DFS Used: 4096 (4 KB)
DFS Used%: 0.00%
Under replicated blocks: 0
Blocks with corrupt replicas: 0
Missing blocks: 0
Missing blocks (with replication factor 1): 0
Pending deletion blocks: 0

-------------------------------------------------
Live datanodes (1):

Name: 10.168.137.12:50010 (hdfs-dn-0.hdfs-dn.test-test.svc.cluster.local)
Hostname: hdfs-dn-0.hdfs-dn.test-test.svc.cluster.local
Decommission Status : Normal
Configured Capacity: 18238930944 (16.99 GB)
DFS Used: 4096 (4 KB)
Non DFS Used: 2086506496 (1.94 GB)
DFS Remaining: 16152420352 (15.04 GB)
DFS Used%: 0.00%
DFS Remaining%: 88.56%
Configured Cache Capacity: 0 (0 B)
Cache Used: 0 (0 B)
Cache Remaining: 0 (0 B)
Cache Used%: 100.00%
Cache Remaining%: 0.00%
Xceivers: 1
Last contact: Mon May 27 12:05:35 UTC 2019
Last Block Report: Mon May 27 11:56:08 UTC 2019
```

### 2. You can list the yarn nodes by running this command:
```
[root@10 svc]#  kubectl -n test-test exec -it yarn-rm-0 -- /usr/local/hadoop/bin/yarn node -list
19/05/27 12:06:33 INFO client.RMProxy: Connecting to ResourceManager at yarn-rm/10.168.239.10:8032
Total Nodes:2
         Node-Id             Node-State Node-Http-Address       Number-of-Running-Containers
yarn-nm-0.yarn-nm.test-test.svc.cluster.local:46318             RUNNING yarn-nm-0.yarn-nm.test-test.svc.cluster.local:8042                                 0
yarn-nm-1.yarn-nm.test-test.svc.cluster.local:36189             RUNNING yarn-nm-1.yarn-nm.test-test.svc.cluster.local:8042                                 0
[root@10 svc]#
```
				  
### 3. Create a port-forward to the yarn resource manager UI:
```
[root@10 svc]#  kubectl -n test-test port-forward yarn-rm-0 8088:8088
Forwarding from 127.0.0.1:8088 -> 8088
Handling connection for 8088
```

### 4. You can run included hadoop tests like this:
```
[root@10 ~]# kubectl -n test-test exec -it yarn-nm-0 -- /usr/local/hadoop/bin/hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-2.9.0-tests.jar TestDFSIO -write -nrFiles 5 -fileSize 128MB -resFile /tmp/TestDFSIOwrite.txt
19/05/27 12:13:31 INFO fs.TestDFSIO: TestDFSIO.1.8
19/05/27 12:13:31 INFO fs.TestDFSIO: nrFiles = 5
19/05/27 12:13:31 INFO fs.TestDFSIO: nrBytes (MB) = 128.0
19/05/27 12:13:31 INFO fs.TestDFSIO: bufferSize = 1000000
19/05/27 12:13:31 INFO fs.TestDFSIO: baseDir = /benchmarks/TestDFSIO
19/05/27 12:13:33 INFO fs.TestDFSIO: creating control file: 134217728 bytes, 5 files
19/05/27 12:13:34 INFO fs.TestDFSIO: created control files for: 5 files
19/05/27 12:13:34 INFO client.RMProxy: Connecting to ResourceManager at yarn-rm/10.168.239.10:8032
19/05/27 12:13:35 INFO client.RMProxy: Connecting to ResourceManager at yarn-rm/10.168.239.10:8032
19/05/27 12:13:35 INFO mapred.FileInputFormat: Total input files to process : 5
19/05/27 12:13:35 INFO mapreduce.JobSubmitter: number of splits:5
19/05/27 12:13:35 INFO Configuration.deprecation: io.bytes.per.checksum is deprecated. Instead, use dfs.bytes-per-checksum
19/05/27 12:13:35 INFO Configuration.deprecation: yarn.resourcemanager.system-metrics-publisher.enabled is deprecated. Instead, use yarn.system-metrics-publisher.enabled
19/05/27 12:13:36 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1558958066937_0001
19/05/27 12:13:37 INFO impl.YarnClientImpl: Submitted application application_1558958066937_0001
19/05/27 12:13:37 INFO mapreduce.Job: The url to track the job: http://yarn-rm-0.yarn-rm.test-test.svc.cluster.local:8088/proxy/application_1558958066937_0001/
19/05/27 12:13:37 INFO mapreduce.Job: Running job: job_1558958066937_0001
19/05/27 12:13:52 INFO mapreduce.Job: Job job_1558958066937_0001 running in uber mode : false
19/05/27 12:13:52 INFO mapreduce.Job:  map 0% reduce 0%
19/05/27 12:14:13 INFO mapreduce.Job:  map 40% reduce 0%
19/05/27 12:14:25 INFO mapreduce.Job:  map 60% reduce 0%
19/05/27 12:14:27 INFO mapreduce.Job:  map 80% reduce 0%
19/05/27 12:14:39 INFO mapreduce.Job:  map 100% reduce 0%
19/05/27 12:14:40 INFO mapreduce.Job:  map 100% reduce 100%
19/05/27 12:14:41 INFO mapreduce.Job: Job job_1558958066937_0001 completed successfully
19/05/27 12:14:41 INFO mapreduce.Job: Counters: 50
        File System Counters
                FILE: Number of bytes read=426
                FILE: Number of bytes written=1222379
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=1165
                HDFS: Number of bytes written=671088717
                HDFS: Number of read operations=23
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=7
        Job Counters
                Killed map tasks=1
                Launched map tasks=5
                Launched reduce tasks=1
                Rack-local map tasks=5
                Total time spent by all maps in occupied slots (ms)=61246
                Total time spent by all reduces in occupied slots (ms)=13232
                Total time spent by all map tasks (ms)=61246
                Total time spent by all reduce tasks (ms)=13232
                Total vcore-milliseconds taken by all map tasks=61246
                Total vcore-milliseconds taken by all reduce tasks=13232
                Total megabyte-milliseconds taken by all map tasks=62715904
                Total megabyte-milliseconds taken by all reduce tasks=13549568
        Map-Reduce Framework
                Map input records=5
                Map output records=25
                Map output bytes=370
                Map output materialized bytes=450
                Input split bytes=605
                Combine input records=0
                Combine output records=0
                Reduce input groups=5
                Reduce shuffle bytes=450
                Reduce input records=25
                Reduce output records=5
                Spilled Records=50
                Shuffled Maps =5
                Failed Shuffles=0
                Merged Map outputs=5
                GC time elapsed (ms)=2574
                CPU time spent (ms)=6990
                Physical memory (bytes) snapshot=1680044032
                Virtual memory (bytes) snapshot=11723608064
                Total committed heap usage (bytes)=1145044992
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=560
        File Output Format Counters
                Bytes Written=77
19/05/27 12:14:42 INFO fs.TestDFSIO: ----- TestDFSIO ----- : write
19/05/27 12:14:42 INFO fs.TestDFSIO:             Date & time: Mon May 27 12:14:42 UTC 2019
19/05/27 12:14:42 INFO fs.TestDFSIO:         Number of files: 5
19/05/27 12:14:42 INFO fs.TestDFSIO:  Total MBytes processed: 640
19/05/27 12:14:42 INFO fs.TestDFSIO:       Throughput mb/sec: 83.41
19/05/27 12:14:42 INFO fs.TestDFSIO:  Average IO rate mb/sec: 95.91
19/05/27 12:14:42 INFO fs.TestDFSIO:   IO rate std deviation: 36.43
19/05/27 12:14:42 INFO fs.TestDFSIO:      Test exec time sec: 67.65
19/05/27 12:14:42 INFO fs.TestDFSIO:
[root@10 ~]#
```

### 5. You can list the mapreduce jobs like this:
```
[root@10 ~]# kubectl -n test-test exec -it yarn-rm-0 -- /usr/local/hadoop/bin/mapred job -list
19/05/27 14:56:02 INFO client.RMProxy: Connecting to ResourceManager at yarn-rm/10.168.137.26:8032
Total jobs:1
                  JobId      State           StartTime      UserName           Queue      Priority       UsedContainers  RsvdContainers  UsedMem  RsvdMem         NeededMem         AM info
 job_1558966828772_0003       PREP       1558968958355          root         default       DEFAULT                    1               0    2048M       0M             2048M      http://yarn-rm-0.yarn-rm.test-test.svc.cluster.local:8088/proxy/application_1558966828772_0003/
[root@10 ~]#
```


### 6. check hadoop ui:
```
1) check yarn-rm ui:
[root@10 ~]# kubectl port-forward hdfs-nn-0 8088:8088

or visit by ingress controller:
[root@10 ~]# curl http://10.1.10.102:33001/cluster

visit: http://58.16.78.136:11111/cluster


2) check hdfs-nn ui:
[root@10 ~]# kubectl port-forward hdfs-nn-0 50070:50070

or visit via ingress controller:
[root@10 ~]# curl http://10.1.10.102:33002/dfshealth.html

visit: http://58.16.78.136:11112/dfshealth.html#tab-overview

3) check hdfs-dn ui:
[root@10 ~]# kubectl port-forward hdfs-dn-0 50075:50075

or visit via ingress controller:
[root@10 ~]# curl http://10.1.10.102:33003/datanode.html
```