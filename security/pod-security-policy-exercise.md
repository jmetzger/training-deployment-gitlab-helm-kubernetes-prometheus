# Exercise for Pod Security Policy 

## Vorbereitung 

```
Die Umgebung muss aufgesetzt, dass PodSecurityPolicy als Admission Controller läuft.
(dazu unter microk8s -> /var/snap/microk8s/currents/args/kubeapi-server die Zeile:
--admission-controller=PodSecurityPolicy hinzugefügt.

In der folgenden Umgebung, ist dies bereis durchgeführt 
# Es gibt die Möglichkeit dieses Script zu verwenden um in DigitalOcean eine Umgebung aufzusetzen.
# Dies kann aber letztendlich auf jedem beliebigen Ubuntu 20.04. LTS Systems erfolgen
```

```
#!/bin/bash  

groupadd sshadmin
USERS="11trainingdo"
for USER in $USERS
do
  echo "Adding user $USER"
  useradd -s /bin/bash $USER
  usermod -aG sshadmin $USER
  echo "$USER:mein-super-geheimes-passwort" | chpasswd
done

# We can sudo with 11trainingdo
usermod -aG sudo 11trainingdo 

# Setup ssh stuff 
sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
usermod -aG sshadmin root
echo "AllowGroups sshadmin" >> /etc/ssh/sshd_config 
systemctl reload sshd 

# Now let us do some generic setup 
echo "Installing microk8s"

# This only works with 1.23 out of the box 
# in 1.24 no secrets for service - accounts are created by default 
snap install --classic --channel=1.23/stable microk8s

microk8s enable dns rbac   
echo "alias kubectl='microk8s kubectl'" >> /root/.bashrc 
source ~/.bashrc 
alias kubectl='microk8s kubectl'

# Installing nfs-common 
apt-get -y update 
apt-get -y install nfs-common jq

git clone https://github.com/jmetzger/lab-microk8s-pod-security-policies lab 

# set the correct rolebinding for the service-account 
cd /lab 

# Create namespace for testing 
microk8s kubectl create namespace sample-psp
microk8s kubectl apply -f rbac/cluster-role-binding-default-sa-at-kube-system-as-cluster-admin.yml 
# Create a developer role in that namespace 
microk8s kubectl apply -f  rbac/default-sa-at-example-psp-namespace.yaml
# helper - sample psp 
helper/create_sa_kubeconfig.sh default sample-psp

# now we need to modify the setting of kube-api-server 
# currently in 1.23 no other admission-plugins are activated  
echo "--enable-admission-plugins=PodSecurityPolicy" >> /var/snap/microk8s/current/args/kube-apiserver 
microk8s stop  
microk8s start 
```

## Übung 1 (nginx erstellen, läuft dieser ?) 

```
# mit dem Server per ssh verbinden 
# zu Testzwecken verzichten wir hier auf einen extra Client 
# sudo -i 
cd 
mkdir manifests 
# vi 01-nginx.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4

```

```
kubectl apply -f 01-nginx.yml
# sind pods gestartet 
kubectl get deploy,rs,pods
# rs - sollte nur einen geben  
kubectl describe rs 
# Pod wurde nicht gestartet:
# Error creating: pods "nginx-deployment-8647b96f59-" is forbidden: PodSecurityPolicy: no providers available to validate pod request
# PodSecurityPolicy ist aktiv, yeah. !! aber es gibt für den user der hier arbeitet keine podsecurity policy die greift 

# Welcher user arbeitet hier - standardmäßig default
kubectl config current-context 

# context: microk8s, user: admin, authentifizierung läuft über token 
kubectl config view

# Alles auf Anfang 
kubectl delete -f 01-pod.yml 
```

## Übung 2a: Set psp (restrictive), clusterrole, clusterrolebinding and try to deploy nginx 

```
# Schritt 1:
# psp erstellen 
# mkdir psp-restrictive 
# cd psp-restrictive 
# vi 01-psp.yml 
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restrictive
spec:
  privileged: false
  hostNetwork: false
  allowPrivilegeEscalation: false
  defaultAllowPrivilegeEscalation: false
  hostPID: false
  hostIPC: false
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  volumes:
  - 'configMap'
  - 'downwardAPI'
  - 'emptyDir'
  - 'persistentVolumeClaim'
  - 'secret'
  - 'projected'
  allowedCapabilities:
  - '*'
  
```

```
kubectl apply -f 01-psp.yml
```

```
# psp does not work without
# 1. a role/clusterrole 
# 2. a rolebind / clusterrolebinding
# Let's start with 1. 
# vi 02-clusterrole.yml 
# Decides which policy to use
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-restrictive
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - restrictive
  verbs:
  - use
```

```
kubectl apply -f 02-clusterrole.yml 
```

```
# Now we need to define, how uses this clusterrole 
# In our case all users, that belong to the Group system::users 
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-restrictive
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - restrictive
  verbs:
  - use
```

```
kubectl apply -f 03-clusterrolebinding.yml 
```

```
# Now deploy an nginx server 
# vi 04-nginx-deploy.yml 
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-restrictive
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - restrictive
  verbs:
  - use
```

```
kubectl apply -f 04-nginx-deploy.yml 
# IT works 
kubectl get deploy,rs,pods
```

## Step 2b: Deploy nginx and WANTING access to host network 

  * Need Step 2a to be done firstly 

```
# vi 05-nginx-host.yml 
# With access to host 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment-host
  labels:
    app: nginx-host
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-host
  template:
    metadata:
      labels:
        app: nginx-host
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
      hostNetwork: true


```

```
# to easier see nginx-deployment-host
kubectl delete -f 04-nginx.host.yml 
kubectl apply -f 05-nginx-host.yml 
# Does not work, why ? 
kubectl get deployment,rs.pods 
```

```
# HostNetwork is in the way 
kubectl describe rs 
# See also - Got the point 
kubectl get psp restrictive -o yaml 
kubectl describe psp restrictive 
kubectl describe psp restrictive | grep "Host Network" 
```
```
# Cleanup 
cd
cd manifests/psp-restrictive 
kubectl delete -f . 

```



## Übung 3: psp (restrictive), clusterrole, rolebinding 

```
# Schritt 1: Create permissive psp 
cd 
cd manifests
mkdir psp-permissive 
cd psp-permissive 
```

```
# vi 01-psp.yml 
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: permissive
spec:
  privileged: true
  hostNetwork: true
  hostIPC: true
  hostPID: true
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  hostPorts:
  - min: 0
    max: 65535
  volumes:
  - '*'
```
  
```
# apply that
kubectl apply -f 01-psp.yml 
kubectl get psp 
```

```
# Schritt 2: Create a clusterrole 
# create a clusterrole 
# vi 02-clusterrole.yml 
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-permissive
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - permissive
  verbs:
  - use
```

```
kubectl apply -f 02-clusterrole.yml 
kubectl get clusterrole 
```

```
# Schritt 3: clusterrolebinding anlegen 
# create clusterrolebinding but only for use default in kube-system 
# vi 03-clusterrolebinding-kube-system.yml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: default-sa-at-kube-system-as-cluster-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: default
  namespace: kube-system
```

```
kubectl apply -f 03-clusterrolebinding-kube-system.yml
```

```
# Schritt 4: Testing Host Network - Anforderung 
# same as in last exercise
# vi 05-nginx-host.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment-host
  labels:
    app: nginx-host
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-host
  template:
    metadata:
      labels:
        app: nginx-host
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
      hostNetwork: true
```

```
# Apply but not to kube-system namespace
kubectl -n kube-system apply -f 05-nginx-host.yml 
# This work, and we want to figure out, if hostname isset. 
kubectl -n kube-system get pods | grep nginx 

```
