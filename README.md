# Deployment und Handling von Applikationen mit Kubernetes, Helm, Prometheus und Gitlab

## Agenda 
  
  1. Kubernetes (Refresher) 
     * [Aufbau von Kubernetes](kubernetes/architecture.md) 
     * Kubernetes und seine Objekte (pods, replicasets, deployments, services, ingress) 
     * Verbinde mit kubectl 
     * Manifeste ausrollen (im Namespace) (2-3)
     * Arbeiten mit non-root images 

 1. gitlab ci/cd 
     * [Overview](/gitlab/01-overview.md)
     * [Using the test - template](/gitlab/02-example-testtemplate.md)
     * [Examples running stages](/gitlab/03-examples-running-stages.md) 
     * [Predefined Vars](/gitlab/04-predefined-vars.md)
     * [Rules](/gitlab/05-rules.md)
     * [Example Defining and using artifacts](/gitlab/06-example-defining-and-using-artifacts.md)

  1. gitlab / Kubernetes (gitops) 
     * [gitlab Kubernetes Agent with gitops - mode](/gitlab/example-gitlab-kubernetes-agent-with-gitops-mode.md)  

  1. gitlab / Kubernetes (CI/CD - Auto Devops) 
     * [Was ist Auto DevOps](/gitlab-ci-cd/was-ist-autodevops.md)

     
     * [Debugging KUBE_CONTEXT - Community Edition](kubernetes-gitlab-ci-cd/04-fix-problem-context-auto-devops.md)

  1. Helm 

  1. Prometheus 
  
  1. Tipps & Tricks 
     * [Default namespace von kubectl ändern](/kubectl/change-default-namespace.md)
     * [Ingress Controller auf DigitalOcean aufsetzen](/digitalocean/ingress-controller-aufsetzen-mit-helm.md)

