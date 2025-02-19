## check pods
```
kubectl get pod 
NAME                                                     READY   STATUS    RESTARTS         AGE
alertmanager-prometheus-kube-prometheus-alertmanager-0   2/2     Running   12 (18m ago)     7d22h
prometheus-grafana-78dd74577f-lk7bz                      3/3     Running   25 (6m58s ago)   7d22h
prometheus-kube-prometheus-operator-5455d847cb-qmbjp     1/1     Running   11 (17m ago)     7d22h
prometheus-kube-state-metrics-7446b747c6-tz7dv           1/1     Running   10 (17m ago)     7d22h
prometheus-prometheus-kube-prometheus-prometheus-0       2/2     Running   12 (18m ago)     7d22h
prometheus-prometheus-node-exporter-4dq8j                1/1     Running   6 (18m ago)      7d22h
```


## check helm install pach
```
helm list --all-namespaces
NAME      	NAMESPACE	REVISION	UPDATED                                  	STATUS  	CHART                       	APP VERSION
my-mysql  	default  	1       	2025-02-10 23:00:57.841724735 +0330 +0330	deployed	mysql-1.6.9                 	5.7.30     
myapp     	default  	1       	2025-02-11 00:07:21.320232533 +0330 +0330	failed  	mychart1-0.1.0              	1.16.0     
prometheus	default  	1       	2025-02-11 22:07:34.742709278 +0330 +0330	deployed	kube-prometheus-stack-69.2.2	v0.80.0    
```
## un install
```
helm uninstall prometheus -n default
release "prometheus" uninstalled
```
