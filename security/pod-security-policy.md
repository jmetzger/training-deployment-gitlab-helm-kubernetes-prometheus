# PodSecurityPolicy 

## Welches Objekt ?

  * kubectl api-resources | grep -i podsecuritypolicy
  * short: psp 
   
## Namespacefähig ?

  * Nein 

## Aktivieren (das reicht nicht) 

  * Der AdmissionController=podSecurityPolicy muss aktiviert sein, dies ist z.B. bei DOKS (Digital Ocean Kubernetes nicht der Fall) 
  * Wenn er nicht aktiviert ist, greift das angelegte Objekt nicht 
  * Aktivierung in microk8s 
  
```
# find / -name "kube-apiserver"
# ${SNAP_DATA}/args/kube-apiserver
# --enable-admission-plugins="PodSecurityPolicy"
microk8s stop 
microk8s start 

# Ref:
# https://microk8s.io/docs/configuring-services

```

## Aktivieren (so geht's) 

```



Hintergründe:
https://kubernetes.io/docs/concepts/security/pod-security-policy/#troubleshooting

```


## Important 

  * podSecurityPolicy works ClusterWide, so we need to authorize some users 
  * who are able to edit the policies 


```
# There is really a clusterrole that can is called edit and can edit 
# So we need some one, who is allowed to do so. 
kubectl get clusterrole | grep ^edit
#edit                                                                   2022-05-08T06:51:30Z

```



```
# https://kubernetes.io/docs/concepts/policy/pod-security-policy/
---
apiVersion: v1
kind: Namespace
metadata:
  name: pod-security-policy-psp-namespace
---
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: pod-security-policy-psp
spec:
  privileged: false  # Don't allow privileged pods!
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  volumes:
    - '*'
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: pod-security-policy-user
  namespace: pod-security-policy-psp-namespace
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-security-policy-psp-user-editor
  namespace: pod-security-policy-psp-namespace
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: edit
subjects:
  - kind: ServiceAccount
    name: pod-security-policy-psp-namespace
    namespace: pod-security-policy-psp-namespace
---
apiVersion: v1
kind: Pod
metadata:
  name: pause
  namespace: pod-security-policy-psp-namespace-unprivileged
spec:
  containers:
    - name: pause
      image: k8s.gcr.io/pause
---
apiVersion: v1
kind: Pod
metadata:
  name: pause
  namespace: pod-security-policy-psp-namespace-privileged
spec:
  containers:
    - name: pause
      image: k8s.gcr.io/pause
      securityContext:
        privileged: true


```

## Ref:

  * https://k8s-examples.container-solutions.com/examples/PodSecurityPolicy/PodSecurityPolicy.html
