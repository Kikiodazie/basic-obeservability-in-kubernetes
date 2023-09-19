# basic-obeservability-in-kubernetes
This repo is for my Open Source Summit Europe 2023 talk on "Understanding Observability in Kubernetes". Schedule link: https://sched.co/1OGeT

## Goal of this short demo
We want to see the resource utilization of pods and nodes in the Killercoda Kubernetes playground cluster.


### See all pods in a cluster
```
kubectl get pods --all-namespaces
```

To get more information on each of the pods you can use the following commands:
- `describe`
- `logs`

List all the events in the default namespace:

```
kubectl get events -n default -o wide

```

To use the `top` command to monitor node and Pod resource utilization:

```
kubectl top 

```

```
kubectl top node
```

```
kubectl top pods -n kube-system
```

Need to configure the metrics server. You can also use more complete metrics pipelines, such as Prometheus.

## deploy metrics server

```

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

```
### See that the metric server has been deployed
```
k get pods -n kube-system
```

## edit metrics server deployment by adding command

```

k -n kube-system edit deploy metrics-server

```


```
containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        command:
        - /metrics-server
        - --kubelet-insecure-tls
        - --kubelet-preferred-address-types=InternalIP

```

### See that the metric server has been redeployed
```
k get pods -n kube-system
```
