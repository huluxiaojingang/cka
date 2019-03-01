

create deployment
```
apiVersion: v1
kind: Deployment
metadata:
  name: rcsise
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sise
  template:
    metadata:
      name: somename
      labels:
        app: sise
    spec:
      containers:
      - name: sise
        image: mhausenblas/simpleservice:0.5.0
        ports:
        - containerPort: 9876
```

create service
```
apiVersion: v1
kind: Service
metadata:
  name: simpleservice
spec:
  ports:
    - port: 80
      targetPort: 9876
  selector:
    app: sise
```


get/describe 
```
Pod IP: 10.244.1.24
Service ClusterIP: 10.107.29.123 

curl 10.107.29.123:80/info
curl 10.244.1.24:9876/info

# iptables config the rules forward the traffic to the pod
iptables-save | grep simpleservice

```

scale

```
kubectl scale deployment rcsise --replicas=2
```

```
kubectl delete svc simpleservice
kubectl delete deployment rcsise
 
```








