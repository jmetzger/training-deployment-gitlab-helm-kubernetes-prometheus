# Debugging KUBE_CONFIG

## Why ? 

```
kubectl does not work, because KUBECONFIG is not set properly  

```

## Find out the context (without setting it)

```
# This overwrites auto devops completely 
#.gitlab-ci.yml 
deploy:
  image:
    name: bitnami/kubectl:latest
    entrypoint: [""]
  script:
    - set
    - kubectl config get-contexts
```

## Find out the context 

```
# This overwrites auto devops completely 
#.gitlab-ci.yml 
deploy:
  image:
    name: bitnami/kubectl:latest
    entrypoint: [""]
  script:
    - set
    - kubectl config get-contexts
# this will be the repo and the name of the agent 
# Take it from the last block 
# you will see it from the pipeline 
    - kubectl config use-context dummyhoney/tln1:gitlab-tln1
    - kubectl get pods
    - ls -la
    - id
```

## Fix by setting KUBE_CONFIG 

```
# This is a problem in the community edition (CE) 
# We need to fix it like so.
# Adjust it to your right context
# IN Settings -> CI/CD -> Variables 
KUBE_CONFIG dummyhoney/spring-autodevops-tln1:gitlab-devops-tn1 

```
