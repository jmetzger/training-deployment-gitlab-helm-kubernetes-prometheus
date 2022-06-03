# Exporter and mongodb 

## prometheus - export 

 * https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-mongodb-exporter


## Step 1: mongodb - deployment in mongodb namespace

```
# vi mongo-db-deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo
        ports:
        - containerPort: 27017
---
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017        
```

```
kubectl apply -f mongo-db-deploy.yml
```


## Step 2: Install prometheus - mongodb - export 

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm show values prometheus-community/prometheus-mongodb-exporter > values.yml

# adjust so it looks like so:
vi values.yml 
# [mongodb[+srv]://][user:pass@]host1[:port1][,host2[:port2],...][/database][?options]
# mongodb-service is the service name
mongodb:
  uri: "mongodb://mongodb-service:27017"

serviceMonitor:
  additionalLabels:
    release: prometheus 
```

```
helm install mongodb-exporter prometheus-community/prometheus-mongodb-exporter -f values.yml
```

## Step 3: Helm -> template -> What does it do ? 

```
helm template mongodb-exporter prometheus-community/prometheus-mongodb-exporter -f values.yml
```
