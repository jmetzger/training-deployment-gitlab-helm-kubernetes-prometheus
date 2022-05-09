# Deployment und Handling von Applikationen mit Kubernetes, Helm, Prometheus und Gitlab

## Agenda 
  
  1. gitlab ci/cd 
     * [Overview](/gitlab/01-overview.md)
     * [Using the test - template](/gitlab/02-example-testtemplate.md)
     * [Examples running stages](/gitlab/03-examples-running-stages.md) 
     * [Predefined Vars](/gitlab/04-predefined-vars.md)
     * [Rules](/gitlab/05-rules.md)
     * [Example Defining and using artifacts](/gitlab/06-example-defining-and-using-artifacts.md)
  
  1. Kubernetes (Refresher) 
     * Aufbau von Kubernetes 
     * Kubernetes und seine Objekte (pods, replicasets, deployments, services, ingress) 
     * Verbinde mit kubectl 
     * Manifeste ausrollen (im Namespace) (2-3)
     * Arbeiten mit non-root images 

  1. gitlab / Kubernetes (gitops) 
     * [gitlab Kubernetes Agent with gitops - mode](/gitlab/example-gitlab-kubernetes-agent-with-gitops-mode.md)  

  1. gitlab / Kubernetes (CI/CD - Auto Devops) 
     * [Was ist Auto DevOps](/gitlab-ci-cd/was-ist-autodevops.md)

  1. Helm 

  1. Prometheus 

## Backlog

```
  1. Grundlagen Kubernetes / Docker (Refresher)  
     * Aufbau von Docker (short)
     * Aufbau von Kubernetes 
     * Kubernetes und seine Objekte (pods, replicasets, deployments, services, ingress) 
     * Verbinde mit kubectl 
     * Aufbau von manifesten
     * Beispiel-Manifeste ausrollen (im Namespace) (2-3)
     * Arbeiten mit non-root images 
   
  1. Kubernetes Praxis / Hintergrund I 
     * Hintergründe Kubernetes Cluster aufsetzen (Gcloud, AWS, DigitalOcean, Vanilla)   
     * Neue Instanzen / Nodes hinzufügen (Beispiel DigitalOcean, microk8s) 
     
  1. gitlab / CI/CD - Pipeline 
     * Die CI/CD - Pipeline - Hintergründe  
     * Stages   
     * Artifacts
     * Dependencies 
     * 

  1. gitlab, Kubernetes und der Agent  
     * Hintergründe zur Integration von Kubernetes (Theorie)
     * Vorstellung Kubernetes Agents und kas 
     * Einrichten des gitlab kubernetes agents (helm)   
     * gitlab ssh-keys einrichten (Optional)  

  1. gitlab - Stages, Artifacts and Dependencies 

o Placing jobs into stages
o Using cache 
o Defining and using artifacts 
o Gitlab Auto DevOps 

o Working with Helm
o Using Auto Devops 
o Configuring gitlab-runner
o Adding repository 
o Automated deployment 

o Deploying to kubernetes 
o Debugging 
o Handling errors
o Testing 

o Helm package manager as a Continous Integration (CI) / Continous Deployment (CD) Tool 
o Installing and Configuration Kubernetes and Helm 
o Refresher on Kubernetes Cluster Architecture and Docker 
o Overview of Helm Features and Architecture 
o Navigating the kubernetes dashboard and helm cli 

o Setting up helm repository (Optional)
o Creating a helm chart 
o Deploying a Kubernetes Application 
o Managing the application 
o Handling Namespaces 

o Monitoring and Logging 
o Securing Helm: Authentication and Authorization 
o Managing the life cycle with hooks 
o Testing helm 

o Working with Database and API's 
o Deploying Complex Applications to Production 
o Troubleshooting 
o Setting up prometheus
o Overview of Prometheus Features and Architecture
o Prometheus as a time series database 

o Use cases for prometheus 
o The prometheus data model 
o Querying the database 
o Service Discovery 
o Monitoring core system components 

o Setting up alerts 
o creating dashboards
o Advanced Querying 
o Instrumenting Services
o Best practces .

```

## Themensammlung 
  
  1. gitlab ci/cd 
     * [Overview](/gitlab/01-overview.md)
     * [Using the test - template](/gitlab/02-example-testtemplate.md)
     * [Examples running stages](/gitlab/03-examples-running-stages.md) 
     * [Predefined Vars](/gitlab/04-predefined-vars.md)
     * [Rules](/gitlab/05-rules.md)
     * [Example Defining and using artifacts](/gitlab/06-example-defining-and-using-artifacts.md)
  1. gitlab / kubernetes (gitops) 
     * [gitlab Kubernetes Agent with gitops - mode](/gitlab/example-gitlab-kubernetes-agent-with-gitops-mode.md)  


