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


