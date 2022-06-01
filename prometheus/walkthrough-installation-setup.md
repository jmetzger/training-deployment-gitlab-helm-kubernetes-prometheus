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

### Step 3a Let's explain (der Prometheus - Server)  

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

# Und vereinfacht (jetzt sehen wir direkt die beiden verwendeten images)
# 1) prometheus - server
# 2) der dazugehörige config-reloader als Side-Car 
kubectl get sts -n prometheus prometheus-prometheus-kube-prometheus-prometheus -o jsonpath='{.spec.template.spec.containers[*].image}'

# Aber wer managed den server -> managed-by -> kubernetes-operator 
kubectl get sts -n prometheus prometheus-prometheus-kube-prometheus-prometheus -o jsonpath="{.spec.template.metadata.labels}" | jq .

# Wir der sts von helm erstellt ? 
# NEIN ;o) 
# show us all the template that helm generate to apply them to kube-api-server 
helm template prometheus prometheus-community/kube-prometheus-stack > all-prometheus.yml 
# NOPE -> none 
cat all-prometheus.yaml | grep -i kind: | grep -i stateful


```

## Step 3b: Prometheus Operator und Admission Controller -> Hook 

```
# The Prometheus Operator for Kubernetes 
# provides easy monitoring definitions 
# for Kubernetes services and deployment and management of Prometheus instances.

# But how are they created 
# After installation new resource-type are introduced 
cat all-prometheus.yaml | grep ^kind: | grep -e 'Prometheus' -e 'ServiceM' | uniq
kind: Prometheus
kind: PrometheusRule
kind: ServiceMonitor
```

## Step 3c: How are the StatefulSets created

```
# New custom resource definitions are created 
# The Prometheus custom resource definition (CRD) declaratively defines a desired Prometheus setup to run in a Kubernetes cluster. It provides options to # configure replication, persistent storage, and Alertmanagers to which the deployed Prometheus instances send alerts to.

# For each Prometheus resource, the Operator deploys a properly configured StatefulSet in the same namespace. The Prometheus Pods are configured to mount # ca Secret called <prometheus-name> containing the configuration for Prometheus.
# https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/crds/crd-prometheuses.yaml

```

## Step 3d: How are PrometheusRules created 

```
# PrometheusRule are manipulated by the MutationHook when they enter the AdmissionController
# The AdmissionController is used after proper authentication in the kube-api-server

cat all-prometheus.yml | grep 'Mutating' -B1 -A32
```

```
# Output 
# Ref: https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/
apiVersion: admissionregistration.k8s.io/v1
kind: MutatingWebhookConfiguration
metadata:
  name:  prometheus-kube-prometheus-admission
  labels:
    app: kube-prometheus-stack-admission    
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/instance: prometheus
    app.kubernetes.io/version: "35.4.2"
    app.kubernetes.io/part-of: kube-prometheus-stack
    chart: kube-prometheus-stack-35.4.2
    release: "prometheus"
    heritage: "Helm"
webhooks:
  - name: prometheusrulemutate.monitoring.coreos.com
    failurePolicy: Ignore
    rules:
      - apiGroups:
          - monitoring.coreos.com
        apiVersions:
          - "*"
        resources:
          - prometheusrules
        operations:
          - CREATE
          - UPDATE
    clientConfig:
      service:
        namespace: prometheus
        name: prometheus-kube-prometheus-operator
        path: /admission-prometheusrules/mutate
    admissionReviewVersions: ["v1", "v1beta1"]
    sideEffects: None
```







## Reference:

  * Techworld with Nana: [https://www.youtube.com/watch?v=QoDqxm7ybLc](https://youtu.be/QoDqxm7ybLc?t=190)
