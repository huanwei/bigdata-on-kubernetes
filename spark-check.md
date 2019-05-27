## NOTES:

### 1. check spark pods
```
[root@10 ~]# kubectl get pods -n test-test -owide
NAME                            READY   STATUS    RESTARTS   AGE     IP              NODE                NOMINATED NODE
hdfs-dn-0                       1/1     Running   0          4h37m   10.168.137.23   10.1.11.75-share    <none>
hdfs-nn-0                       1/1     Running   0          4h43m   10.168.137.21   10.1.11.75-share    <none>
spark-master-55cd7b955b-6smzv   1/1     Running   0          6m27s   10.168.137.35   10.1.11.75-share    <none>
spark-worker-555775464b-j6j4t   1/1     Running   0          18s     10.168.137.41   10.1.11.75-share    <none>
spark-worker-555775464b-jgmq7   1/1     Running   0          18s     10.168.137.39   10.1.11.75-share    <none>
spark-worker-555775464b-z8vqr   1/1     Running   0          18s     10.168.137.40   10.1.11.75-share    <none>
yarn-nm-0                       1/1     Running   0          3h11m   10.168.137.28   10.1.11.75-share    <none>
yarn-nm-1                       1/1     Running   0          3h11m   10.168.111.53   10.1.10.105-share   <none>
yarn-rm-0                       1/1     Running   0          3h43m   10.168.137.26   10.1.11.75-share    <none>
```

### 2. check spark worker log
```
[root@10 ~]# kubectl -n test-test logs -f spark-worker-555775464b-z8vqr
19/05/28 02:04:00 INFO Worker: Registered signal handlers for [TERM, HUP, INT]
19/05/28 02:04:01 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
19/05/28 02:04:02 INFO SecurityManager: Changing view acls to: root
19/05/28 02:04:02 INFO SecurityManager: Changing modify acls to: root
19/05/28 02:04:02 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: Set(root); users with modify permissions: Set(root)
19/05/28 02:04:04 INFO Slf4jLogger: Slf4jLogger started
19/05/28 02:04:04 INFO Remoting: Starting remoting
19/05/28 02:04:05 INFO Remoting: Remoting started; listening on addresses :[akka.tcp://sparkWorker@10.168.137.40:46286]
19/05/28 02:04:05 INFO Utils: Successfully started service 'sparkWorker' on port 46286.
19/05/28 02:04:06 INFO Worker: Starting Spark worker 10.168.137.40:46286 with 8 cores, 1024.0 MB RAM
19/05/28 02:04:06 INFO Worker: Running Spark version 1.5.1
19/05/28 02:04:06 INFO Worker: Spark home: /opt/spark
19/05/28 02:04:06 INFO Utils: Successfully started service 'WorkerUI' on port 8080.
19/05/28 02:04:06 INFO WorkerWebUI: Started WorkerWebUI at http://10.168.137.40:8080
19/05/28 02:04:06 INFO Worker: Connecting to master master:7077...
19/05/28 02:04:08 INFO Worker: Successfully registered with master spark://master:7077
^C
```

### 3. check spark master log
```
[root@10 spark]# kubectl -n test-test logs -f spark-master-55cd7b955b-6smzv
19/05/27 23:56:04 INFO Master: Registered signal handlers for [TERM, HUP, INT]
19/05/27 23:56:05 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
19/05/27 23:56:06 INFO SecurityManager: Changing view acls to: root
19/05/27 23:56:06 INFO SecurityManager: Changing modify acls to: root
19/05/27 23:56:06 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: Set(root); users with modify permissions: Set(root)
19/05/27 23:56:08 INFO Slf4jLogger: Slf4jLogger started
19/05/27 23:56:08 INFO Remoting: Starting remoting
19/05/27 23:56:09 INFO Remoting: Remoting started; listening on addresses :[akka.tcp://sparkMaster@master:7077]
19/05/27 23:56:09 INFO Utils: Successfully started service 'sparkMaster' on port 7077.
19/05/27 23:56:10 INFO Master: Starting Spark master at spark://master:7077
19/05/27 23:56:10 INFO Master: Running Spark version 1.5.1
19/05/27 23:56:11 INFO Utils: Successfully started service 'MasterUI' on port 8080.
19/05/27 23:56:11 INFO MasterWebUI: Started MasterWebUI at http://10.168.212.70:8080
19/05/27 23:56:11 INFO Utils: Successfully started service on port 6066.
19/05/27 23:56:11 INFO StandaloneRestServer: Started REST server for submitting applications on port 6066
19/05/27 23:56:12 INFO Master: I have been elected leader! New state: ALIVE
19/05/28 00:39:47 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:39:54 INFO Master: 10.168.212.76:41830 got disassociated, removing it.
19/05/28 00:39:54 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.212.76:41830] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:39:54 INFO Master: 10.168.212.76:41830 got disassociated, removing it.
19/05/28 00:40:05 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:40:12 INFO Master: 10.168.212.76:46030 got disassociated, removing it.
19/05/28 00:40:12 INFO Master: 10.168.212.76:46030 got disassociated, removing it.
19/05/28 00:40:12 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.212.76:46030] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:40:21 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:40:35 INFO Master: 10.168.111.55:43200 got disassociated, removing it.
19/05/28 00:40:35 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.111.55:43200] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:40:35 INFO Master: 10.168.111.55:43200 got disassociated, removing it.
19/05/28 00:40:40 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:40:43 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:40:51 INFO Master: 10.168.212.76:39412 got disassociated, removing it.
19/05/28 00:40:51 INFO Master: 10.168.212.76:39412 got disassociated, removing it.
19/05/28 00:40:51 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.212.76:39412] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:40:54 INFO Master: 10.168.111.55:33710 got disassociated, removing it.
19/05/28 00:40:54 INFO Master: 10.168.111.55:33710 got disassociated, removing it.
19/05/28 00:40:54 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.111.55:33710] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:41:04 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:41:13 INFO Master: 10.168.137.29:46071 got disassociated, removing it.
19/05/28 00:41:13 INFO Master: 10.168.137.29:46071 got disassociated, removing it.
19/05/28 00:41:13 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.137.29:46071] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:41:14 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:41:23 INFO Master: 10.168.111.55:44281 got disassociated, removing it.
19/05/28 00:41:23 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.111.55:44281] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:41:23 INFO Master: 10.168.111.55:44281 got disassociated, removing it.
19/05/28 00:41:27 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:41:31 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:41:31 INFO Master: 10.168.137.29:38813 got disassociated, removing it.
19/05/28 00:41:31 INFO Master: 10.168.137.29:38813 got disassociated, removing it.
19/05/28 00:41:31 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.137.29:38813] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:41:37 INFO Master: 10.168.212.76:37715 got disassociated, removing it.
19/05/28 00:41:37 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.212.76:37715] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:41:37 INFO Master: 10.168.212.76:37715 got disassociated, removing it.
19/05/28 00:41:56 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:41:57 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:42:03 INFO Master: 10.168.111.55:34708 got disassociated, removing it.
19/05/28 00:42:03 INFO Master: 10.168.111.55:34708 got disassociated, removing it.
19/05/28 00:42:03 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.111.55:34708] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:42:08 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:42:21 INFO Master: 10.168.137.29:33239 got disassociated, removing it.
19/05/28 00:42:21 INFO Master: 10.168.137.29:33239 got disassociated, removing it.
19/05/28 00:42:21 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.137.29:33239] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:42:33 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:42:39 INFO Master: 10.168.212.76:45014 got disassociated, removing it.
19/05/28 00:42:39 INFO Master: 10.168.212.76:45014 got disassociated, removing it.
19/05/28 00:42:39 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.212.76:45014] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:42:52 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:42:58 ERROR ErrorMonitor: dropping message [class akka.actor.ActorSelectionMessage] for non-local recipient [Actor[akka.tcp://sparkMaster@spark-master:7077/]] arriving at [akka.tcp://sparkMaster@spark-master:7077] inbound addresses are [akka.tcp://sparkMaster@master:7077]
akka.event.Logging$Error$NoCause$
19/05/28 00:42:58 INFO Master: 10.168.111.55:44615 got disassociated, removing it.
19/05/28 00:42:58 INFO Master: 10.168.111.55:44615 got disassociated, removing it.
19/05/28 00:42:58 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.111.55:44615] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:43:08 INFO Master: 10.168.137.29:46571 got disassociated, removing it.
19/05/28 00:43:08 INFO Master: 10.168.137.29:46571 got disassociated, removing it.
19/05/28 00:43:08 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.137.29:46571] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:54:49 INFO Master: Registering worker 10.168.111.56:37621 with 4 cores, 1024.0 MB RAM
19/05/28 00:55:07 INFO Master: Registering worker 10.168.212.77:36035 with 8 cores, 1024.0 MB RAM
19/05/28 00:55:30 INFO Master: Registering worker 10.168.137.30:41341 with 8 cores, 1024.0 MB RAM
19/05/28 00:55:47 INFO Master: 10.168.137.30:41341 got disassociated, removing it.
19/05/28 00:55:47 INFO Master: Removing worker worker-20190528005353-10.168.137.30-41341 on 10.168.137.30:41341
19/05/28 00:55:47 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.137.30:41341] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:55:47 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.111.56:37621] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:55:47 INFO Master: 10.168.137.30:41341 got disassociated, removing it.
19/05/28 00:55:47 INFO Master: 10.168.111.56:37621 got disassociated, removing it.
19/05/28 00:55:47 INFO Master: Removing worker worker-20190528005348-10.168.111.56-37621 on 10.168.111.56:37621
19/05/28 00:55:47 INFO Master: 10.168.111.56:37621 got disassociated, removing it.
19/05/28 00:55:47 INFO Master: 10.168.212.77:36035 got disassociated, removing it.
19/05/28 00:55:47 INFO Master: Removing worker worker-20190528005330-10.168.212.77-36035 on 10.168.212.77:36035
19/05/28 00:55:47 INFO Master: 10.168.212.77:36035 got disassociated, removing it.
19/05/28 00:55:47 WARN ReliableDeliverySupervisor: Association with remote system [akka.tcp://sparkWorker@10.168.212.77:36035] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
19/05/28 00:55:57 INFO Master: Registering worker 10.168.111.57:41742 with 4 cores, 1024.0 MB RAM
19/05/28 00:55:58 INFO Master: Registering worker 10.168.212.78:41299 with 8 cores, 1024.0 MB RAM
19/05/28 00:56:00 INFO Master: Registering worker 10.168.137.31:37200 with 8 cores, 1024.0 MB RAM


```

### 4. execute spark task
```
[root@10 ~]# kubectl -n test-test exec -it  spark-worker-555775464b-z8vqr bash
root@spark-worker-555775464b-z8vqr:/# spark-submit  --master spark://master:7077 /opt/spark/examples/src/main/python/pi.py
19/05/28 02:05:29 INFO SparkContext: Running Spark version 1.5.1
19/05/28 02:05:31 INFO SecurityManager: Changing view acls to: root
19/05/28 02:05:31 INFO SecurityManager: Changing modify acls to: root
19/05/28 02:05:31 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: Set(root); users with modify permissions: Set(root)
19/05/28 02:05:34 INFO Slf4jLogger: Slf4jLogger started
19/05/28 02:05:34 INFO Remoting: Starting remoting
19/05/28 02:05:35 INFO Remoting: Remoting started; listening on addresses :[akka.tcp://sparkDriver@10.168.137.40:46870]
19/05/28 02:05:35 INFO Utils: Successfully started service 'sparkDriver' on port 46870.
19/05/28 02:05:35 INFO SparkEnv: Registering MapOutputTracker
19/05/28 02:05:35 INFO SparkEnv: Registering BlockManagerMaster
19/05/28 02:05:35 INFO DiskBlockManager: Created local directory at /tmp/blockmgr-0a49cf1f-611c-401a-8790-ba35752803f3
19/05/28 02:05:36 INFO MemoryStore: MemoryStore started with capacity 530.0 MB
19/05/28 02:05:36 INFO HttpFileServer: HTTP File server directory is /tmp/spark-c86705ea-ef32-469c-8ef9-e5df00568e1b/httpd-7e7fe2f4-f8c3-4af6-a7a5-e74f89ecef2c
19/05/28 02:05:36 INFO HttpServer: Starting HTTP Server
19/05/28 02:05:36 INFO Utils: Successfully started service 'HTTP file server' on port 37276.
19/05/28 02:05:36 INFO SparkEnv: Registering OutputCommitCoordinator
19/05/28 02:05:37 INFO Utils: Successfully started service 'SparkUI' on port 4040.
19/05/28 02:05:37 INFO SparkUI: Started SparkUI at http://10.168.137.40:4040
19/05/28 02:05:38 INFO Utils: Copying /opt/spark/examples/src/main/python/pi.py to /tmp/spark-c86705ea-ef32-469c-8ef9-e5df00568e1b/userFiles-407e539a-e2f9-4748-9e1e-725359b3a6c4/pi.py
19/05/28 02:05:38 INFO SparkContext: Added file file:/opt/spark/examples/src/main/python/pi.py at http://10.168.137.40:37276/files/pi.py with timestamp 1558980338832
19/05/28 02:05:39 INFO AppClient$ClientEndpoint: Connecting to master spark://master:7077...
19/05/28 02:05:41 INFO SparkDeploySchedulerBackend: Connected to Spark cluster with app ID app-20190528020541-0000
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor added: app-20190528020541-0000/0 on worker-20190528020405-10.168.137.40-46286 (10.168.137.40:46286) with 8 cores
19/05/28 02:05:41 INFO SparkDeploySchedulerBackend: Granted executor ID app-20190528020541-0000/0 on hostPort 10.168.137.40:46286 with 8 cores, 1024.0 MB RAM
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor added: app-20190528020541-0000/1 on worker-20190528020405-10.168.137.39-41466 (10.168.137.39:41466) with 8 cores
19/05/28 02:05:41 INFO SparkDeploySchedulerBackend: Granted executor ID app-20190528020541-0000/1 on hostPort 10.168.137.39:41466 with 8 cores, 1024.0 MB RAM
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor added: app-20190528020541-0000/2 on worker-20190528020405-10.168.137.41-36082 (10.168.137.41:36082) with 8 cores
19/05/28 02:05:41 INFO SparkDeploySchedulerBackend: Granted executor ID app-20190528020541-0000/2 on hostPort 10.168.137.41:36082 with 8 cores, 1024.0 MB RAM
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor updated: app-20190528020541-0000/0 is now RUNNING
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor updated: app-20190528020541-0000/1 is now RUNNING
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor updated: app-20190528020541-0000/2 is now RUNNING
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor updated: app-20190528020541-0000/2 is now LOADING
19/05/28 02:05:41 INFO AppClient$ClientEndpoint: Executor updated: app-20190528020541-0000/1 is now LOADING
19/05/28 02:05:42 INFO AppClient$ClientEndpoint: Executor updated: app-20190528020541-0000/0 is now LOADING
19/05/28 02:05:42 INFO Utils: Successfully started service 'org.apache.spark.network.netty.NettyBlockTransferService' on port 42848.
19/05/28 02:05:42 INFO NettyBlockTransferService: Server created on 42848
19/05/28 02:05:42 INFO BlockManagerMaster: Trying to register BlockManager
19/05/28 02:05:42 INFO BlockManagerMasterEndpoint: Registering block manager 10.168.137.40:42848 with 530.0 MB RAM, BlockManagerId(driver, 10.168.137.40, 42848)
19/05/28 02:05:42 INFO BlockManagerMaster: Registered BlockManager
19/05/28 02:05:44 INFO SparkDeploySchedulerBackend: SchedulerBackend is ready for scheduling beginning after reached minRegisteredResourcesRatio: 0.0
19/05/28 02:05:48 INFO SparkContext: Starting job: reduce at /opt/spark/examples/src/main/python/pi.py:39
19/05/28 02:05:48 INFO DAGScheduler: Got job 0 (reduce at /opt/spark/examples/src/main/python/pi.py:39) with 2 output partitions
19/05/28 02:05:48 INFO DAGScheduler: Final stage: ResultStage 0(reduce at /opt/spark/examples/src/main/python/pi.py:39)
19/05/28 02:05:48 INFO DAGScheduler: Parents of final stage: List()
19/05/28 02:05:48 INFO DAGScheduler: Missing parents: List()
19/05/28 02:05:48 INFO DAGScheduler: Submitting ResultStage 0 (PythonRDD[1] at reduce at /opt/spark/examples/src/main/python/pi.py:39), which has no missing parents
19/05/28 02:05:50 INFO MemoryStore: ensureFreeSpace(4080) called with curMem=0, maxMem=555755765
19/05/28 02:05:50 INFO MemoryStore: Block broadcast_0 stored as values in memory (estimated size 4.0 KB, free 530.0 MB)
19/05/28 02:05:50 INFO MemoryStore: ensureFreeSpace(2737) called with curMem=4080, maxMem=555755765
19/05/28 02:05:50 INFO MemoryStore: Block broadcast_0_piece0 stored as bytes in memory (estimated size 2.7 KB, free 530.0 MB)
19/05/28 02:05:50 INFO BlockManagerInfo: Added broadcast_0_piece0 in memory on 10.168.137.40:42848 (size: 2.7 KB, free: 530.0 MB)
19/05/28 02:05:50 INFO SparkContext: Created broadcast 0 from broadcast at DAGScheduler.scala:861
19/05/28 02:05:50 INFO DAGScheduler: Submitting 2 missing tasks from ResultStage 0 (PythonRDD[1] at reduce at /opt/spark/examples/src/main/python/pi.py:39)
19/05/28 02:05:50 INFO TaskSchedulerImpl: Adding task set 0.0 with 2 tasks
19/05/28 02:05:51 INFO SparkDeploySchedulerBackend: Registered executor: AkkaRpcEndpointRef(Actor[akka.tcp://sparkExecutor@10.168.137.41:44889/user/Executor#559116768]) with ID 2
19/05/28 02:05:52 WARN TaskSetManager: Stage 0 contains a task of very large size (365 KB). The maximum recommended task size is 100 KB.
19/05/28 02:05:52 INFO TaskSetManager: Starting task 0.0 in stage 0.0 (TID 0, 10.168.137.41, PROCESS_LOCAL, 374514 bytes)
19/05/28 02:05:52 INFO TaskSetManager: Starting task 1.0 in stage 0.0 (TID 1, 10.168.137.41, PROCESS_LOCAL, 502317 bytes)
19/05/28 02:05:53 INFO BlockManagerMasterEndpoint: Registering block manager 10.168.137.41:38555 with 530.0 MB RAM, BlockManagerId(2, 10.168.137.41, 38555)
19/05/28 02:05:54 INFO SparkDeploySchedulerBackend: Registered executor: AkkaRpcEndpointRef(Actor[akka.tcp://sparkExecutor@10.168.137.39:38541/user/Executor#-536510652]) with ID 1
19/05/28 02:05:55 INFO BlockManagerMasterEndpoint: Registering block manager 10.168.137.39:40786 with 530.0 MB RAM, BlockManagerId(1, 10.168.137.39, 40786)
19/05/28 02:05:57 INFO BlockManagerInfo: Added broadcast_0_piece0 in memory on 10.168.137.41:38555 (size: 2.7 KB, free: 530.0 MB)
19/05/28 02:05:59 INFO SparkDeploySchedulerBackend: Registered executor: AkkaRpcEndpointRef(Actor[akka.tcp://sparkExecutor@10.168.137.40:33338/user/Executor#-999915360]) with ID 0
19/05/28 02:06:00 INFO BlockManagerMasterEndpoint: Registering block manager 10.168.137.40:42424 with 530.0 MB RAM, BlockManagerId(0, 10.168.137.40, 42424)
19/05/28 02:06:00 INFO TaskSetManager: Finished task 1.0 in stage 0.0 (TID 1) in 8600 ms on 10.168.137.41 (1/2)
19/05/28 02:06:00 INFO TaskSetManager: Finished task 0.0 in stage 0.0 (TID 0) in 8888 ms on 10.168.137.41 (2/2)
19/05/28 02:06:00 INFO TaskSchedulerImpl: Removed TaskSet 0.0, whose tasks have all completed, from pool
19/05/28 02:06:00 INFO DAGScheduler: ResultStage 0 (reduce at /opt/spark/examples/src/main/python/pi.py:39) finished in 9.699 s
19/05/28 02:06:00 INFO DAGScheduler: Job 0 finished: reduce at /opt/spark/examples/src/main/python/pi.py:39, took 12.495738 s
Pi is roughly 3.135860
19/05/28 02:06:01 INFO SparkUI: Stopped Spark web UI at http://10.168.137.40:4040
19/05/28 02:06:01 INFO DAGScheduler: Stopping DAGScheduler
19/05/28 02:06:01 INFO SparkDeploySchedulerBackend: Shutting down all executors
19/05/28 02:06:01 INFO SparkDeploySchedulerBackend: Asking each executor to shut down
19/05/28 02:06:01 INFO MapOutputTrackerMasterEndpoint: MapOutputTrackerMasterEndpoint stopped!
19/05/28 02:06:01 INFO MemoryStore: MemoryStore cleared
19/05/28 02:06:01 INFO BlockManager: BlockManager stopped
19/05/28 02:06:01 INFO BlockManagerMaster: BlockManagerMaster stopped
19/05/28 02:06:01 INFO OutputCommitCoordinator$OutputCommitCoordinatorEndpoint: OutputCommitCoordinator stopped!
19/05/28 02:06:01 INFO SparkContext: Successfully stopped SparkContext
19/05/28 02:06:01 INFO RemoteActorRefProvider$RemotingTerminator: Shutting down remote daemon.
19/05/28 02:06:01 INFO RemoteActorRefProvider$RemotingTerminator: Remote daemon shut down; proceeding with flushing remote transports.
19/05/28 02:06:02 INFO RemoteActorRefProvider$RemotingTerminator: Remoting shut down.
19/05/28 02:06:02 INFO ShutdownHookManager: Shutdown hook called
19/05/28 02:06:02 INFO ShutdownHookManager: Deleting directory /tmp/spark-c86705ea-ef32-469c-8ef9-e5df00568e1b/pyspark-92a2d98a-cd1c-4f38-9015-9753358d1031
19/05/28 02:06:02 INFO ShutdownHookManager: Deleting directory /tmp/spark-c86705ea-ef32-469c-8ef9-e5df00568e1b
root@spark-worker-555775464b-z8vqr:/#
```

