

pv use hostPath
```
kind: PersistentVolume
apiVersion: v1
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

pvc

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: manual
  
```

kubectl get pvc Status Bound

pv & pvc bound conditions:
- storage 
- accessModes
- storageClassName

Pod:
```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: pv-deploy
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: mypv
    spec:
      containers:
      - name: shell
        image: centos:7
        command:
        - "bin/bash"
        - "-c"
        - "sleep 10000"
        volumeMounts:
        - name: mypd
          mountPath: "/tmp/persistent"
      volumes:
      - name: mypd
        persistentVolumeClaim:
          claimName: myclaim
```

Delete the pod and the deployment will create new pod.  Enter the new container and the file exists.
Also,the file exist on the node.

```
kubectl delete deploy pv-deploy
kubectl delete pvc myclaim
```





