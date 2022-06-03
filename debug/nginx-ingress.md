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
helm template gitlab2 -n gitlab --create-namespace=gitlab -f values-rbac.yml gitlab/gitlab > manifests-rbac-gitlab.yml 

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


## Reference 

  * https://kubernetes.github.io/ingress-nginx/troubleshooting/
