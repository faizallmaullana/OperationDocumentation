# Longhorn


### Install Longhorn

```sh
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.5.1/deploy/longhorn.yaml
```


### Run Temporary Longhorn
```sh
kubectl run longhorn-writer \
  -n suarai-backend \
  --image=alpine \
  --restart=Never \
  --command -- sleep 3600
````

```yaml
# longhorn-writer.yaml
apiVersion: v1
kind: Pod
metadata:
  name: longhorn-writer
  namespace: suarai-backend
spec:
  containers:
    - name: writer
      image: ubuntu
      command: ["sleep", "3600"]
      volumeMounts:
        - name: suarai-source
          mountPath: /app/bin
  volumes:
    - name: suarai-source
      persistentVolumeClaim:
        claimName: suarai-source-pvc
```

### Coppy to Longhorn

```sh
kubectl cp /home/ubuntu/my-binary suarai-backend/longhorn-writer:/data/my-binary
kubectl exec -n suarai-backend longhorn-writer -- chmod +x /data/my-binary
```

### certmanager

```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml
```
