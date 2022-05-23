# Übung PodAffinity/PodAntiAffinity 

## Übung 1: PodAffinity (forced) - auf gleicher Node/Hostname

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

## Übung 2: PodAffinity (forced) - im gleichen Rack 

```
# Schritt 1:
# Bei einem cluster, dieser Schritt nur durch trainer 
kubectl get nodes --show-labels
kubectl label nodes pool-tg5g9rh4y-cw8mb rack=1
kubectl label nodes pool-tg5g9rh4y-cw8mr rack=1
kubectl label nodes pool-tg5g9rh4y-cw8mw rack=2

```

```
# Schritt 2.
# mkdir pod-affinity-racktest
# cd pod-affinity-racktest
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
# Schritt 3: 
kubectl apply -f 01-busybox-sleep.yml 
```

```
# Schritt 4:
# Welche Hostnamen gibt es. 
# Wichtig, um topologyKey zu verstehen:
kubectl get nodes -o yaml | grep hostname
```

```
# Schritt 5:
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
            topologyKey: rack

      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

```
# Schritt 6: Prüfen und ausrollen 
# auf welcher node läuft busybox 
kubectl get pods -l app=backend -o wide 

# deployment ausrollen 
kubectl apply -f 02-frontend-nginx.yml

# Wo laufen die Deployments ? 
kubectl get pods -l frontend=nginx -o wide 

# und wir lassen uns nochmal das describe ausgeben 
kubectl describe pods nginx-frontend-<key> 

```





## Übung 3: PodAntiAffinity (forced) auf anderem Hosts 

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
        podAntiAffinity:
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

# Wo laufen die Deployments ? // das muss jetzt auf anderen Hosts sein 
kubectl get pods -l frontend=nginx -o wide 

# und wir lassen uns nochmal das describe ausgeben 
kubectl describe pods nginx-frontend-<key> 

```

## Übung 4: PodAffinity (preferred) - auf gleicher Node/Hostname

```
# Schritt 1.
# mkdir pod-affinity-preferred
# cd pod-affinity-preferred
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
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 80
            podAffinityTerm:
              labelSelector:
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

## Variante 5: PodAffinity (preferred) - auf gleicher Node/Hostname mit matchExpression

```
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
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                - key: app
                  operator: In
                  values:
                  - backend
              topologyKey: kubernetes.io/hostname

      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```
