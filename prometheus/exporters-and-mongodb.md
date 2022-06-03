# Exporter and mongodb 

## prometheus - export 

 * https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-mongodb-exporter


## Step 1: mongodb - deployment in mongodb namespace






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
