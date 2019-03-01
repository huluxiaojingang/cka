

livenessProbe

```
apiVersion: v1
kind: Pod
metadata:
  name: hc
spec:
  containers:
  - name: sise
    image: mhausenblas/simpleservice:0.5.0
    ports:
    - containerPort: 9876
    livenessProbe:
      initialDelaySeconds: 2
      periodSeconds: 5
      httpGet:
        path: /health
        port: 9876

```

bad pod

```
apiVersion: v1
kind: Pod
metadata:
  name: badpod
spec:
  containers:
  - name: sise
    image: mhausenblas/simpleservice:0.5.0
    ports:
    - containerPort: 9876
    env:
    - name: HEALTH_MIN
      value: "1000"
    - name: HEALTH_MAX
      value: "4000"
    livenessProbe:
      initialDelaySeconds: 2
      periodSeconds: 5
      httpGet:
        path: /health
        port: 9876
    restartPolicy: Always
```

describe the pod ,Liveness probe failed , the pod restartes


readinessProbe
```
apiVersion: v1
kind: Pod
metadata:
  name: ready
spec:
  containers:
  - name: sise
    image: mhausenblas/simpleservice:0.5.0
    ports:
    - containerPort: 9876
    readinessProbe:
      initialDelaySeconds: 10
      httpGet:
        path: /health
        port: 9876
```

```
kubectl delete pod/hc pod/ready pod/badpod
```


