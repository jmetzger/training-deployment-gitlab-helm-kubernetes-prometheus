# Examples nginx-controller - connect to kube-api-server

## Nice to know:

```
curl is installed in nginx-controller 
```

## Simplify things 

```
BEARER=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
CA_CERT=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
API_URL=https://kubernetes.default.svc.cluster.local
```

### Getting the api information 

```
# apiVersion: v1 (show all apiversions from core) 
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL/api

# e.g. apiVersion: apps/v1 
# you need to use apis instead of api
# Long Version 
# curl --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt -H  "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://kubernetes.default.svc.cluster.local/apis
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL/apis
```

### Example 1: pods in own namespace (gitlab) and in others + What does ingress need ? 

```
# Get pods from gitlab workspace 
# That is similar to a kubectl get pods -> verb: list  
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL/api/v1/namespaces/gitlab/pods

# Get pods from any workspace - e.g. default 
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL/api/v1/namespaces/default/pods

# Get pods from any workspace 
# This should be possible by cluster role:
kubectl get clusterroles gitlab-nginx-ingress
NAME                   CREATED AT
gitlab-nginx-ingress   2022-06-02T17:03:30Z

# gitlab-nginx-ingress 
# Question is: how is ->gitlab<- constructed here ? Is it the release ? or the namespace ? 
curl --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt -H  "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://kubernetes.default.svc.cluster.local/api/v1/namespaces/default/pods

```

### Example 2: 

```
# what clusterroles -> permissions are needed
kubectl get clusterroles gitlab-nginx-ingress -o yaml | grep -A 90 'name: gitlab-nginx-ingress' 

# Specifically
- apiGroups:
  - networking.k8s.io
  resources:
  - ingressclasses
  verbs:
  - get
  - list
  - watch

# So lets check 
# ingressclasses -> list 
# apis -> is needed here 

# Already set in example 1
BEARER=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
CA_CERT=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
API_URL=https://kubernetes.default.svc.cluster.local

# STEP 1: show us all versions of networking.k8s.io
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL/apis/networking.k8s.io

# STEP 2: show us all kinds of networking.k8s.io/v1 
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL/apis/networking.k8s.io/v1/
```

#### STEP 3: Validate -> ingressclasses -> list 

```
ACTION=/apis/networking.k8s.io/v1/ingressclasses 
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL$ACTION
```

#### STEP 4: Validate -> ingressclasses -> get 

```
ACTION=/apis/networking.k8s.io/v1/ingressclasses/gitlab-nginx 
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL$ACTION
```

#### STEP 5: Validate -> ingressclass -> watch 

```
ACTION=/apis/networking.k8s.io/v1/watch/ingressclasses/gitlab-nginx
curl --cacert $CA_CERT -H  "Authorization: Bearer $BEARER" $API_URL$ACTION
```
 

