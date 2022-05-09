# Using Kubernetes with gitlab/gitops

## Create a new project 

```
  * Name: kubernetes-gitops-tn<nr>
  * e.g. k8s-gitops-tn1 
  * Public 
  * Readme.md 
  * Disabled -> SAST 
```

## Setting up the config (gitops - Style) - sample not yet working 

  * Create an agent configuration file 
  * .gitlab/agents/name/ 
  * We will use the following convention or name in the training:
    * gitlab-agent-tn-nr- - gitlab-agent-tn1  
  * https://docs.gitlab.com/ee/user/clusters/agent/install/index.html#create-an-agent-configuration-file
  * Then in that folder we need to place a configuration - file - config.yaml - NOT !!! - config.yml 
    * THE CONFIGURATION WILL NOT GET DETECTED 
  * Content see below: 

```
# gitops:
# tln1 ersetzen, durch eigene teilnemer - nr. bei default_namespace 
gitops:
  manifest_projects:
  - id: dummyhoney/kubernetes-gitops-tn1
    default_namespace: tln1
    paths:
      # Read all YAML/YML  files from this directory. 
    - glob: '/manifests/**/*.{yaml,yml}'
    # Read all .yaml files from team2/apps and all subdirectories.
    # - glob: '/team2/apps/**/*.yaml'
    # If 'paths' is not specified or is an empty list, the configuration below is used.
    # - glob: '/**/*.{yaml,yml,json}'
    reconcile_timeout: 3600s
    dry_run_strategy: none
    prune: true
    prune_timeout: 3600s
    prune_propagation_policy: foreground
    inventory_policy: must_match
```

  * Reference: https://docs.gitlab.com/ee/user/clusters/agent/gitops.html


## Connect the cluster under Infrastructure -> Kubernetes
  
  * Select the agent and click Register 
  * Copy the token to clipboard 
  
## Install the agent in the cluster using your client (Linux) 
  
```
# Install helm3 if not present yet.
# Ubuntu Style:
snap install --classic helm 
# kubectl needs to be working properly: kubectl cluster-info 
# Can you see the cluster ?
helm repo add gitlab https://charts.gitlab.io
helm repo update
# adjust namespace tn<teilnehmer> e.g. gitlab-agent-tn1 
helm upgrade --install gitlab-agent gitlab/gitlab-agent \
    --namespace gitlab-agent-tn1 \
    --create-namespace \
    --set config.token=<config-token-you-copied to clipboard> \
    --set config.kasAddress=wss://kas.gitlab.com  
  
  
```  
  
## Check if it has been registered 

  * Look into infrastructure - kubernetes   
  
  
  
## Creating sample manifests file 

```
# manifests/project1/web/bitnami-nginx-deploy.yml 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80



```



## Checking the logs 
 
```
kubectl logs -n gitlab-agent-tn1 deploy/gitlab-agent
 
 
