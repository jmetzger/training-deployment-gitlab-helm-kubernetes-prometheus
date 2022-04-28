# Kubernetes und Docker Administration und Orchestrierung 

## Agenda

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
     * Ausblick AutoScaling (Ops) 

  1. Kubernetes Storage 
     * Grundlagen (Dev/Ops)
     * Objekte PersistantVolume / PersistantVolumeClaim (Dev/Ops) 
     * [Praxis. Beispiel (Dev/Ops)](/shared-volumes/nfs.md) 

  1. Kubernetes Networking 
     * [Überblick](/kubernetes-networks/overview.md) 
     * Pod to Pod
     * Webbasierte Dienste (Ingress) 
     * IP per Pod
     * Inter Pod Communication ClusterDNS 

  1. Kubernetes Paketmanagement (Helm) 
     * Warum ? (Dev/Ops)
     * Grundlagen / Aufbau / Verwendung (Dev/Ops)
     * [Praktisches Beispiel bitnami/mysql (Dev/Ops)](/helm/example.md) 

  1. Kubernetes Rechteverwaltung (RBAC) 
     * Warum ? (Ops)
     * Rollen und Rollenzuordnung (Ops)
     * Service Accounts (Ops)
     * [Praktische Umsetzung anhand eines Beispiels (Ops)](/kubernetes/rbac-create-user.md)

  1. Kubernetes Monitoring 
     * Protokollieren mit Elasticsearch und Fluentd (Devs/Ops)
     * [Long Installation step-by-step - Digitalocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes)
     * Container Level Monitoring (Devs/Ops)
     * [Setting up metrics-server - microk8s](/microk8s/metrics-server.md)
     * Prometheus/cAdvisor (Devs/Ops)
     * InfluxDB (Ops) 

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
     * [Ingress -> Nginx Proxy](/kubernetes/ingress.md)

  1. Kubernetes - RBAC 
     * [Nutzer einrichten](/kubernetes/rbac-create-user.md) 
 
  1. Kubernetes - Netzwerk (CNI's) 
     * [Übersicht Netzwerke](/kubernetes-networks/overview.md) 
     * [Callico - nginx example](/kubernetes-network/callico/00-simple-example.md)
     * [Callico - client-backend-ui-example](/kubernetes-network/callico/01-example-with-services.md)
   
  1. kubectl 
     * [Bash completion for kubectl](/kubectl/bash-completion.md)
     * [kubectl Spickzettel](/kubectl/spickzettel.md)
     * [Tipps&Tricks zu Deploymnent - Rollout](/kubectl/rollout.md) 
     
  1. kubectl - manifest - examples 
     * [02 Pod nginx mit Port und IP innerhalb des Clusters](/kubectl-examples/02-pod-nginx-exposed.md)
     * [03b Example with service and nginx](/kubectl-examples/03b-service.md)
     * [04 Ingress mit einfachem Beispiel](/kubectl-examples/04-ingress-nginx.md)
     * [05 Ingress mit Permanent Redirect](/kubectl-examples/05-ingress-permanent-redirect.md)

  1. Kubernetes - Monitoring (microk8s und vanilla) 
     * [metrics-server aktivieren (microk8s und vanilla)](/microk8s/metrics-server.md)

  1. Kubernetes - Shared Volumes 
     * [Shared Volumes with nfs](shared-volumes/nfs.md)

  1. Kubernetes - Wartung 


  1. Kubernetes - Tipps & Tricks 
     * [Assigning Pods to Nodes](/tipps-tricks/pods-2-nodes.md) 

  1. Kubernetes - Documentation 
     * [Documentation zu microk8s plugins/addons](https://microk8s.io/docs/addons)
     * [Shared Volumes - Welche gibt es ?](https://kubernetes.io/docs/concepts/storage/volumes/)

  1. Linux und Docker Tipps & Tricks allgemein 
     * [vim einrückung für yaml-dateien](/vim/vim-yaml.md)
     * [YAML Linter Online](http://www.yamllint.com/)
     
