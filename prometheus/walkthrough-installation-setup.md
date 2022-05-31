# Walkthrough - Installation (Helm) and Setup 

## Prerequisites

  * Ubuntu 20.04 with running microk8s single cluster 
  * Works on any other cluster, but installing helm is different 

## Prepare 

```
# Be sure helm is installed on your client 
# In our walkthrough, we will do it directly on 1 node, 
# which is not recommended for Production 

```

## Walkthrough 

### Step 1: install helm, if not there yet

```
snap install --classic helm 
```

### Step 2: Rollout prometheus/grafana stack in namespace prometheus 

```
# add prometheus repo 
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# install stack into new prometheus namespace 
helm install -n prometheus --create-namespace prometheus prometheus-community/kube-prometheus-stack

# After installation look at the pods 
# You should see 3 pods 
kubectl --namespace prometheus get pods -l "release=prometheus"

# After a while it should be more pods
kubectl get all -n prometheus

```

### Step 3: Let's explain 

```
# 2 Stateful sets
kubectl get statefulsets  -n prometheus
# output 
# alertmanager-prometheus-kube-prometheus-alertmanager   1/1     5m14s
# prometheus-prometheus-kube-prometheus-prometheus.      1/1.    5m23s
```
```
# Moving part 1: 
# prometheus-prometheus-kube-prometheus-prometheus
# That is the core prometheus server based on the main image 

# Let's validate 
# schauen wir mal in das File 
kubectl get statefulset -n prometheus -o yaml > sts-prometheus-server.yml

# Und vereinfacht (jetzt sehen wir direkt die beiden verwendeten images 
# 1) prometheus - server
# 2) der dazugehörige config-reloader als Side-Car 
kubectl get sts -n prometheus prometheus-prometheus-kube-prometheus-prometheus -o jsonpath='{.spec.template.spec.containers[*].image}'
```



## Reference:

  * Techworld with Nana: [https://www.youtube.com/watch?v=QoDqxm7ybLc](https://youtu.be/QoDqxm7ybLc?t=190)
