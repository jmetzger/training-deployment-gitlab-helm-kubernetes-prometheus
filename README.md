# Deployment und Handling von Applikationen mit Kubernetes, Helm, Prometheus und Gitlab

## Agenda 

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

o Stages, Artifacts and Dependencies 

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


## Themensammlung 
  
  1. gitlab ci/cd 
     * [Overview](/gitlab/overview.md)
     * [Using the test - template](/gitlab/example-testtemplate.md) 
     * [Example Defining and using artifacts](/gitlab/example-defining-and-using-artifacts.md)
  1. gitlab 
     * [gitlab Kubernetes Agent with gitops - mode](/gitlab/example-gitlab-kubernetes-agent-with-gitops-mode.md)  


## Backlog

  1. Kubernetes Grundlagen 
     * [Allgemeine Einführung in Container (Dev/Ops)](overview-docker.md)
     * [Warum Kubernetes ? (Devs/Ops)](warum-kubernetes.md)
     * [Die Struktur von Kubernetes mit seinen Komponenten (Devs/Ops)](/kubernetes/architecture.md) 
     * [Umdenken in der Administration (feste Server vs. Dienste im Cluster) (Ops)](/kubernetes/admin-change.md) 
     * [Api Versionierung Lifetime](/kubernetes/api-versionierung-lifetime.md)

  1. Kubernetes Kickoff 
     * [Orchestrierung (Warum und wozu ?) (Devs/Ops)](orchestrierung.md)
     * [Microservices (Warum ? Wie ?) (Devs/Ops)](microservices.md)
     * [Hochverfügbarkeit (Wie funktioniert das ?) (Ops)](ha.md)
     * [Vorstellung Management - Tools zum Aufsetzen eines Cluster (microk8s,kubeadm,Rancher) (Ops)](microk8s-kubeadm-rancher.md) 

  1. Kubernetes Praxis API-Objekte 
     * [Das Tool kubectl (Devs/Ops)](/kubectl/spickzettel.md)
     * [kubectl example with run](/kubectl/run-with-example.md)
     * Arbeiten mit manifests (Devs/Ops)
     * Pods (Devs/Ops)
     * [kubectl/manifest/pod](/kubectl-examples/01-pod-nginx.md)
     * ReplicaSets (Theorie) - (Devs/Ops)
     * [kubectl/manifest/replicaset](/kubectl-examples/01a-replicaset-nginx.md)
     * Deployments (Devs/Ops)
     * [kubectl/manifest/deployments](/kubectl-examples/03-nginx-deployment.md)
     * Services (Devs/Ops)
     * [kubectl/manifest/service](/kubectl-examples/03b-service.md)
     * DaemonSets (Devs/Ops)
     * IngressController (Devs/Ops)
     * [Hintergrund Ingress](/kubernetes/ingress.md) 
     * [Documentation for default ingress nginx](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/)
     * [Beispiel mit Hostnamen](/kubectl-examples/04-ingress-nginx-with-hostnames.md)

  1. Kubernetes Praxis Scaling/Rolling Updates/Wartung 
     * Rolling Updates (Devs/Ops) 
     * Scaling von Deployments (Devs/Ops) 
     * [Wartung mit drain / uncordon (Ops)](/kubectl/uncordon-drain.md) 
     * [Ausblick AutoScaling (Ops)](/kubernetes/autoscaling.md) 

  1. Kubernetes Storage 
     * Grundlagen (Dev/Ops)
     * Objekte PersistantVolume / PersistantVolumeClaim (Dev/Ops) 
     * [Praxis. Beispiel (Dev/Ops)](/shared-volumes/nfs-multiple.md) 

  1. Kubernetes Networking 
     * [Überblick](/kubernetes-networks/overview.md) 
     * Pod to Pod
     * Webbasierte Dienste (Ingress) 
     * IP per Pod
     * Inter Pod Communication ClusterDNS 
     * [Beispiel NetworkPolicies](/kubernetes-network/callico/00-simple-example-multi.md)

  1. Kubernetes Paketmanagement (Helm) 
     * [Warum ? (Dev/Ops)](helm/warum.md)
     * Grundlagen / Aufbau / Verwendung (Dev/Ops)
     * [Praktisches Beispiel bitnami/mysql (Dev/Ops)](/helm/example.md) 

  1. Kubernetes Rechteverwaltung (RBAC) 
     * Warum ? (Ops)
     * Rollen und Rollenzuordnung (Ops)
     * Service Accounts (Ops)
     * [Praktische Umsetzung anhand eines Beispiels (Ops)](/kubernetes/rbac-create-user-multi.md)

  1. Kubernetes Monitoring 
     * [Ebenen des Loggings](ebenen-des-loggings.md) 
     * [Working with kubectl logs](/kubectl/logs.md)
     * [Built-In Monitoring tools - kubectl top pods/nodes](microk8s/metrics-server.md)
     * [Protokollieren mit Elasticsearch und Fluentd (Devs/Ops)](microk8s/fluent-kibana-elastic-mit-microk8s.md)
     * [Long Installation step-by-step - Digitalocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes)
     * Container Level Monitoring (Devs/Ops)
     * [Setting up metrics-server - microk8s](/microk8s/metrics-server.md)
     * Prometheus/cAdvisor (Devs/Ops)
     * InfluxDB (Ops) 
  
  1. Kubernetes Security 
     * [Grundlagen und Beispiel (Praktisch)](security/grundlagen-security.md)

  1. Kubernetes CI/CD (Optional) 
     * Canary Deployment (Devs/Ops) 
     * Blue Green Deployment (Devs/Ops) 
     * A/B Testing (Devs/Ops) 

  1. Tipps & Tricks 
     * [bash-completion](/kubectl/bash-completion.md) 
     * [kubectl spickzettel](/kubectl/spickzettel.md)
     * [Alte manifests migrieren](/kubectl/convert-plugin.md)
    
  1. Fragen 
     * [Q and A](/q-and-a.md)

## Backlog 

  1. Kubernetes - microk8s (Installation und Management) 
     * [Patch to next major release - cluster](microk8s/patch-next-major.md)
     * [Installation Kuberenetes Dashboard](/microk8s/dashboard.md) 

  1. Kubernetes - API - Objekte
  
     * [Was sind Deployments](/kubernetes/deployments.md)
     * [Service - Objekt und IP](/kubernetes/service.md)

  1. Kubernetes - Netzwerk (CNI's) 
     * [Übersicht Netzwerke](/kubernetes-networks/overview.md) 
     * [Callico - nginx example](/kubernetes-network/callico/00-simple-example.md)
     * [Callico - client-backend-ui-example](/kubernetes-network/callico/01-example-with-services.md)
   
  1. kubectl 
     * [Tipps&Tricks zu Deploymnent - Rollout](/kubectl/rollout.md) 
     
  1. kubectl - manifest - examples 
     * [05 Ingress mit Permanent Redirect](/kubectl-examples/05-ingress-permanent-redirect.md)

  1. Kubernetes - Monitoring (microk8s und vanilla) 
     * [metrics-server aktivieren (microk8s und vanilla)](/microk8s/metrics-server.md)

  1. Kubernetes - Tipps & Tricks 
     * [Assigning Pods to Nodes](/tipps-tricks/pods-2-nodes.md)

  1. Linux und Docker Tipps & Tricks allgemein 
     * [vim einrückung für yaml-dateien](/vim/vim-yaml.md)
     * [YAML Linter Online](http://www.yamllint.com/)
     
