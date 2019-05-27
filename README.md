## Create Hadoop cluster

```
kubectl create -f hadoop/cm/hadoop-configmap.yaml

kubectl create -f hadoop/statefulset/hdfs-dn-statefulset.yaml
kubectl create -f hadoop/statefulset/hdfs-nn-statefulset.yaml
kubectl create -f hadoop/statefulset/yarn-nm-statefulset.yaml
kubectl create -f hadoop/statefulset/yarn-rm-statefulset.yaml

kubectl create -f svc/hdfs-dn-svc.yaml
kubectl create -f svc/hdfs-nn-svc.yaml
kubectl create -f svc/yarn-nm-svc.yaml
kubectl create -f svc/yarn-rm-svc.yaml
kubectl create -f svc/yarn-ui-svc.yaml

```

## Create Spark cluster

```
kubectl create -f spark/spark-master.yaml
kubectl create -f spark/spark-master-svc.yaml
kubectl create -f spark/spark-worker.yaml
kubectl create -f spark/spark-zeppelin.yaml
kubectl create -f spark/spark-zeppelin-svc.yaml
kubectl create -f spark/spark-webui-svc.yaml

```