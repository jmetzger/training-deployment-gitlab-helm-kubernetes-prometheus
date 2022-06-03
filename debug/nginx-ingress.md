# Debugging nginx-ingress

## Step 1: gitlab ausrollen ohne cert-manager 

```
# mkdir manifests/gitlab 
# cd manifests/gitlab
# vi values-rbac.yml 
global:
  ingress:
    configureCertmanager: false
    
certmanager:                                                                                         
  install: false 
```

```
helm template gitlab2 -n gitlab --create-namespace -f values-rbac.yml gitlab/gitlab > manifests-rbac-gitlab.yml 

```

```



```

## Debugging connection to kube-api-server

```
# Step 1: create a namespace 
kubectl create ns web-test 
kubectl config set-context --current --namespace web-test 

# Step 2: Now run a deploymdnt 
kubectl create deployment nginx --image=nginx

# Step 3: 
kubectl describe pods 

# you will see the following lines 
# Mounts:
#   /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-56nvt (ro)
# So the serviceaccount is automatically mounted 

# Let's have a look into it. 
kubectl exec -it  nginx-8f458dc5b-fb96s -- bash
ls -la /var/run/secrets/kubernetes.io/serviceaccount
# output 
#total 4
#drwxrwxrwt 3 root root  140 Jun  2 10:51 .
#drwxr-xr-x 3 root root 4096 Jun  2 10:51 ..
#drwxr-xr-x 2 root root  100 Jun  2 10:51 ..2022_06_02_10_51_30.2232465508
#lrwxrwxrwx 1 root root   32 Jun  2 10:51 ..data -> ..2022_06_02_10_51_30.2232465508
#lrwxrwxrwx 1 root root   13 Jun  2 10:51 ca.crt -> ..data/ca.crt
#lrwxrwxrwx 1 root root   16 Jun  2 10:51 namespace -> ..data/namespace
#lrwxrwxrwx 1 root root   12 Jun  2 10:51 token -> ..data/token

```

```
Step 2: Let us do the connection test 
How ? 
Can we access the needed data. 


```

## Bug  / Troubleshooting 

  * https://github.com/kubernetes/ingress-nginx/issues/4610

```
Closing. Unfortunately, that is not possible. We only receive 403 errors and there is no way to check what RBAC permissions are missing.

@0x53A you can see exactly the error if you add the flag --v=10 in the ingress controller deployment

```

## Vorgehen (Achtung: Bitte nur für Troubleshooting)

```
kubectl get deploy gitlab2-nginx-ingress-controller -o yaml > ingress-controller.yaml 
vi ingress-controller.yaml 
# bei den args neue zeile einfügen
# sachen rausschmeisen sich auf die gespeicherte version etcd -> zB resourceid 
- -v=10
kubectl apply -f ingress-controller.yaml 
kubectl describe po gitlab2-nginx-ingress-controller-848865fc88-6knhb
kubectl logs gitlab2-nginx-ingress-controller-848865fc88-6knhb | less

# now we can see in the logs, that there are problems connecting to kube-api-server
# permissions 
est.go:1123] Response Body: {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"services \"gitlab2-nginx-ingress-defaultbackend\" is forbidden: User \"system:serviceaccount:gitlab:gitlab2-nginx-ingress\" cannot get resource \"services\" in API group \"\" in the namespace \"gitlab\"","reason":"Forbidden","details":{"name":"gitlab2-nginx-ingress-defaultbackend","kind":"services"},"code":403}
F0603 13:31:04.134976       7 main.go:83] 


```



## Reference 

  * https://kubernetes.github.io/ingress-nginx/troubleshooting/
