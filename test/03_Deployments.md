
deployment.yaml

```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: sise-deploy
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: sise
    spec:
      containers:
      - name: sise
        image: mhausenblas/simpleservice:0.5.0
        ports:
        - containerPort: 9876
        env:
        - name: SIMPLE_SERVICE_VERSION
          value: "0.9"

```

kubectl get/describe xxx
```
kubectl get deploy
kubectl get rs
kubectl get pods
```

update version to 1.0

```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: sise-deploy
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: sise
    spec:
      containers:
      - name: sise
        image: mhausenblas/simpleservice:0.5.0
        ports:
        - containerPort: 9876
        env:
        - name: SIMPLE_SERVICE_VERSION
          value: "1.0"
```

```
kubectl apply -f xxx . update deploy

new pods new rs. old pods are Terminating
```

```
curl <pod.ip>:9876/info 
{... "version": "1.0"...}  the version from "0.9" to "1.0"
```

rollout history
```
$ kubectl rollout history deploy/sise-deploy
deployments "sise-deploy"
REVISION        CHANGE-CAUSE
1               <none>
2               <none>
```

roll back
```
kubectl rollout undo deploy/sise-deploy --to-revision=1

```









