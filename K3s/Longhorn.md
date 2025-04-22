# Longhorn

- [Home](../README.md)
- [Instalation](Instalation.md)
- [Longhorn](Longhorn.md)

## Install Longhorn

```sh
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.5.1/deploy/longhorn.yaml
```


## Run Temporary Longhorn

You can use command line or configuration file yaml

### Command Line

```sh
kubectl run longhorn-writer \
  -n suarai-backend \
  --image=alpine \
  --restart=Never \
  --command -- sleep 3600
````

### Configuration file yaml

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

## Copy to Longhorn

Here is how to copy file to inside longhorn

```sh
kubectl cp /home/ubuntu/my-binary suarai-backend/longhorn-writer:/data/my-binary
kubectl exec -n suarai-backend longhorn-writer -- chmod +x /data/my-binary
```

## Longhhorn-ui

Configuration to expose longhorn UI using ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: longhorn-ingress
  namespace: longhorn-system
  annotations:
    nginx.ingress.kubernetes.io/backend-protocol: "HTTP"
spec:
  rules:
  - host:
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: longhorn-frontend
            port:
              number: 80
```