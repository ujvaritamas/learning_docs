# Stern

## Why is it useful
Kubernetes has concept of [replication controller](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/) which ensure that n pods are running at the same time. This allow rolling updates, redundancy. Multiple pods running and all have a unique id. If you want to check the logs you need to know the unique ID every time (what can is changing if the pod restarted ...). If you want to debug an application you need to check the logs of every pod (kubernetes load balances the traffic so you won't know which pod the requests ends up at.)
```
kubectl get pods
kubectl log -f service-1786497219-2rbt1  # pod 1
kubectl log -f service-1786497219-8kfbp  # pod 2
kubectl log -f service-1786497219-lttxd  # pod 3
```

Additionally there can be multiple container inside the pod.
```
kubectl get pods
kubectl log -f service-1786497219-2rbt1 server #pod 1 or kubectl log service-1786497219-2rbt1 -c server
kubectl log -f service-1786497219-2rbt1 gateway  # pod 1
kubectl log -f service-1786497219-8kfbp server   # pod 2
kubectl log -f service-1786497219-8kfbp gateway  # pod 2
...
```

## Stern
It's a utility that allows you to specify both the pod id and the container id as regular expressions. 

`stern service`
This will match any pod containing the word service and listen to all containers within it.  If you only want to see traffic to the server container you could do `stern --container server service` and it'll stream the logs of all the server containers from the 3 pods.   
In addition, if a pod is killed and recreated during a deployment Stern will stop listening to the old pod and automatically hook into the new one. There's no more need to figure out what the id of that newly created pod is.   


Tail the gateway container running inside of the envvars pod on staging  
`stern --context staging --container gateway envvars`


Show auth activity from 15min ago with timestamps  
`stern -t --since 15m auth`


View pods from another namespace  
`stern --namespace kube-system kubernetes-dashboard`


[doc and installation](https://github.com/stern/stern)