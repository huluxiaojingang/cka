
create pod with command
```
kubectl run sise --image=mhausenblas/simpleservice:0.5.0 --port=9876

kubectl get pods

kubect describe pod sise-xxxx | grep IP:
IP:  10.244.1.12

curl 10.244.1.12:9876/info
{"host": "172.17.0.3:9876", "version": "0.5.0", "from": "172.17.0.1"}
```

Pod.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: twocontainers
spec:
  containers:
  - name: sise
    image: mhausenblas/simpleservice:0.5.0
    ports:
    - containerPort: 9876
  - name: shell
    image: centos:7
    command:
      - "bin/bash"
      - "-c"
      - "sleep 10000"
```

进入容器命令。-c 容器名。一个Pod两个容器，他们共享网络、存储。

```
kubectl exec twocontainers -c shell -ti -- bash
进入shell容器，访问sise容器端口，仍然能够访问通
curl localhost:9876/info
```

创建Pod memory 54Mi，cpu 500m
```
apiVersion: v1
kind: Pod
metadata:
  name: constraintpod
spec:
  containers:
  - name: sise
    image: mhausenblas/simpleservice:0.5.0
    ports:
    - containerPort: 9876
    resources:
      limits:
        memory: "64Mi"
        cpu: "500m"
```

delete pod
```
kubectl delete deployment sise
kubectl delete pod twocontainers
kubectl delete pod constraintpod
```










