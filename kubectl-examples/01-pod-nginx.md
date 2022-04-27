# Example: static pod nginx

## Walkthrough 

```
vi nginx-static.yml 

apiVersion: v1
kind: Pod
metadata:
  name: nginx-static-web
  labels:
    webserver: nginx
spec:
  containers:
  - name: web
    image: nginx

```

```
kubectl apply -f nginx-static.yml 
kubectl describe nginx-static-web 
# show config 
kubectl get pod/nginx-static-web -o yml
kubectl get pod/nginx-static-web -o wide 
```
