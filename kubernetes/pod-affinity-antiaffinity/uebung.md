# Übung PodAffinity/PodAntiAffinity 

## Übung 1: PodAffinity - auf gleicher Node/Hostname

```
# Schritt 1.
# mkdir pod-affinity-test
# cd pod-affinity-test
# vi 01-busybox-sleep.yml 
apiVersion: v1
kind: Pod
metadata:
  name: sleep-busybox
  labels:
    app: backend
spec:
  containers:
    - name: bb
      image: busybox 
      command: ["sleep"]
      args: ["999999"]     
```

```
# Schritt 2: 
kubectl apply -f 01-busybox-sleep.yml 
```

```
# Schritt 3:
# Welche Hostnamen gibt es. 
# Wichtig, um topologyKey zu verstehen:
kubectl get nodes -o yaml | grep hostname
```

```
# Schritt 4:
# Deployment mit podAffinity festlegen 
# vi 02-fronted-nginx.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-frontend
spec:
  selector:
    matchLabels:
      frontend: nginx

  replicas: 10
  template:
    metadata:
      labels:
        frontend: nginx
    spec:
      affinity:
        podAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchLabels:
                app: backend
            topologyKey: kubernetes.io/hostname

      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

```
# Schritt 5: Prüfen und ausrollen 
# auf welcher node läuft busybox 
kubectl get pods -l app=backend -o wide 

# deployment ausrollen 
kubectl apply -f 02-fronted-nginx.yml

# Wo laufen die Deployments ? 
kubectl get pods -l frontend=nginx -o wide 

# und wir lassen uns nochmal das describe ausgeben 
kubectl describe pods nginx-frontend-<key> 

```

## Übng 2:






## Übung 2: PodAntiAffinity 

```

```

