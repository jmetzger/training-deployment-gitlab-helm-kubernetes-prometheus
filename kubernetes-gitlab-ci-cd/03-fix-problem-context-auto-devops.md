# Debugging kube_config 

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
    - kubectl config use-context dummyhoney/spring-autodevops-tln1:gitlab-devops-tn1
    - kubectl get pods -n gitlab-agent-tln1
```

## Fix by setting KUBE_CONFIG 

```
# This is a problem in the community edition (CE) 
# We need to fix it like so.
# Adjust it to your right context
# IN Settings -> CI/CD -> Variables 
KUBE_CONFIG dummyhoney/spring-autodevops-tln1:gitlab-devops-tn1 

```
