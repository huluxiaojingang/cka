create pod 
```
apiVersion: v1
kind: Pod
metadata:
  name: labelex
  labels:
    env: development
spec:
  containers:
  - name: sise
    image: mhausenblas/simpleservice:0.5.0
    ports:
    - containerPort: 9876
```

```
kubectl get pods --show-labels
```

add a label to the pod
```
# labelex is the pod name
kubectl label pods labelex owner=michael
```

list pod with label
```
kubectl get pods --selector owner=michael
kubectl get pods -l env=development
```

annother pod
```
apiVersion: v1
kind: Pod
metadata:
  name: labelexother
  labels:
    env: production
    owner: michael
spec:
  containers:
  - name: sise
    image: mhausenblas/simpleservice:0.5.0
    ports:
    - containerPort: 9876
```

list pods with label env=development or env=production
```
kubectl get pods -l 'env in (production,development)'
```

delete
```
kubectl delete pods -l 'env in (production, development)'
kubectl delete pods labelex
kubectl delete pods labelexother

```













