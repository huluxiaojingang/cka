

create secret
```
echo -n "adfg23DSFR5" > ./apikey.txt
kubectl create secret generic apikey --from-file=./apikey.txt
```




```
apiVersion: v1
kind: Pod
metadata:
  name: consumesec
spec:
  containers:
  - name: shell
    image: centos:7
    command:
      - "bin/bash"
      - "-c"
      - "sleep 10000"
    volumeMounts:
      - name: apikeyvol
        mountPath: "/tmp/apikey"
        readOnly: true
  volumes:
  - name: apikeyvol
    secret:
      secretName: apikey
```

```
kubectl exec -it consumesec -c shell -- bash
"/tmp/apikey/apikey.txt"
```


```
kubectl delete pod consumesec
```




