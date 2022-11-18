### Deploy Redis Cluster using Helm Chart

###### To deploy Redis Sentinel using the Helm package manager and the Redis Helm Chart

To install Helm3 use this documentation 

```
https://helm.sh/docs/intro/install/ 
```

###### Add Helm Repo Add and Run Helm

```shell
helm repo add redis https://raw.githubusercontent.com/eyupguner/ha-redis-with-sentinel/main/
```

```shell
helm upgrade --install <release-name> redis/ha-redis-with-sentinel -n <namespace>
```

```shell
helm install redis redis/ha-redis-with-sentinel -n redis
```

###### HA Redis Cluster with Sentinel and Sentinel Proxy
This chart will install ha redis cluster with sentinel on kubernetes. You can reach the source files on  https://github.com/eyupguner/ha-redis-with-sentinel. The Redis will work one master pod and two slaves pods. Slaves pods will replicate from master pod. If master pod crash, sentinels will select new master.  

###### This chart will create following recources.

```shell
- Three Redis Pod
- Three Sentinel Pods 
- Three Redis Pod's PVC called pvc-redis
- Three Sentinel Pod's PVC called pvc-sentinel
- Redis Pod's ConfigMap called redis-config
- Sentinel Proxy Pod and PVC
- 8 ClusterIP Service and 1 NodePort Service
```
Services

```shell
- svc-redis-sentinel-proxy service forwards internal request to only master pod.
- svc-redis-sentinel-proxy-nodeport forwards external request to only master pod.
- Other services forward request to own pods.
```

To test replication
```
$ kubectl exec -it redis-sentinel-1 -n namespace -- sh
redis-cli 
auth admin
info replication
```
Create some key-value pair data
```
SET emp1 raja
SET emp2 mano
SET emp3 ram
```
Now get the key-value pair list
```
$ KEYS *
```
Now log in to the slave pods and check to see if they show the same three data
```
$ kubectl exec -it redis-sentinel-2 -n namespace -- sh
redis-cli 
auth admin
KEYS *
```
