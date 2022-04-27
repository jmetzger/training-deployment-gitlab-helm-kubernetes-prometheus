# Kubernetes und Docker Administration und Orchestrierung 

## Agenda

  1. Kubernetes Grundlagen 
     * [Allgemeine Einführung in Container (Dev/Ops)](overview-docker.md)
     * [Warum Kubernetes ? (Devs/Ops)](warum-kubernetes.md)
     * [Die Struktur von Kubernetes mit seinen Komponenten (Devs/Ops)](/kubernetes/architecture.md) 
     * [Umdenken in der Administration (feste Server vs. Dienste im Cluster) (Ops)](/kubernetes/admin-change.md) 

  1. Kubernetes Kickoff 
     * [Orchestrierung (Warum und wozu ?) (Devs/Ops)](orchestrierung.md)
     * [Microservices (Warum ? Wie ?) (Devs/Ops)](microservices.md)
     * [Hochverfügbarkeit (Wie funktioniert das ?) (Ops)](ha.md)
     * [Vorstellung Management - Tools zum Aufsetzen eines Cluster (microk8s,kubeadm,Rancher) (Ops)](microk8s-kubeadm-rancher.md) 

  1. Kubernetes Praxis API-Objekte 
     * [Das Tool kubectl (Devs/Ops) - mit beispielen (pod starten](kubectl-with-examples.md)
     * [kubectl example with run](/kubectl/run-with-example.md)
     * Arbeiten mit manifests (Devs/Ops)
     * Pods (Devs/Ops)
     * [kubectl/manifest/pod](/kubectl-examples/01-pod-nginx.md)
     * ReplicaSets (Theorie) - (Devs/Ops)
     * 
     * Services (Devs/Ops)
     * Deployments (Devs/Ops)
     * DaemonSets (Devs/Ops)
     * IngressController (Devs/Ops) 

  1. Kubernetes Praxis Scaling/Rolling Updates/Wartung 
     * Rolling Updates (Devs/Ops) 
     * Scaling von Deployments (Devs/Ops) 
     * Wartung mit drain / uncordon (Ops) 
     * Ausblick AutoScaling (Ops) 

  1. Kubernetes Storage 
     * Grundlagen (Dev/Ops)
     * Objekte PersistantVolume / PersistantVolumeClaim (Dev/Ops) 
     * Praxis. Beispiel (Dev/Ops) 

  1. Kubernetes Networking 
     * Pod to Pod
     * Webbasierte Dienste (Ingress) 
     * IP per Pod
     * Inter Pod Communication ClusterDNS 

  1. Kubernetes Paketmanagement (Helm) 
     * Warum ? (Dev/Ops)
     * Grundlagen / Aufbau / Verwendung (Dev/Ops)
     * Praktisches Beispiel (Dev/Ops) 

  1. Kubernetes Rechteverwaltung (RBAC) 
     * Warum ? (Ops)
     * Rollen und Rollenzuordnung (Ops)
     * Service Accounts (Ops)
     * Praktische Umsetzung anhand eines Beispiels (Ops) 

  1. Kubernetes Monitoring 
     * Protokollieren mit Elasticsearch und Fluentd (Devs/Ops)
     * Container Level Monitoring (Devs/Ops)
     * Prometheus/cAdvisor (Devs/Ops)
     * InfluxDB (Ops) 

  1. Kubernetes CI/CD (Optional) 
     * Canary Deployment (Devs/Ops) 
     * Blue Green Deployment (Devs/Ops) 
     * A/B Testing (Devs/Ops) 

  1. Tipps & Tricks 
     * [bash-completion](/kubectl/bash-completion.md) 
     * [kubectl spickzettel](/kubectl/spickzettel.md)

## Backlog 
 
  1. Kubernetes - Überblick
     * [Warum Kubernetes, was macht Kubernetes](warum-kubernetes.md) 
     * [Welches System ? (minikube, micro8ks etc.)](welches-system.md)

  1. Kubernetes - microk8s (Installation und Management) 
     * [Patch to next major release - cluster](microk8s/patch-next-major.md)
     * [Remote-Verbindung zu Kubernetes (microk8s) einrichten](microk8s/connect-from-remote.md)
     * [Create a cluster with microk8s](microk8s/cluster.md)
     * [Ingress controller in microk8s aktivieren](microk8s/ingress.md) 
     * [Arbeiten mit der Registry](microk8s/registry.md)
     * [Installation Kuberenetes Dashboard](/microk8s/dashboard.md) 

  1. Kubernetes - API - Objekte
     * [Welche API-Objekte gibt es? (Kommando)](/kubernetes/api-resources.md)
     * [Api Versionierung Lifetime](/kubernetes/api-versionierung-lifetime.md)
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
     * [Start pod (container with run && examples)](/kubectl/run-with-example.md)
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

  1. Kubernetes - Backups 
     + [Kubernetes Aware Cloud Backup - kasten.io](/backups/cluster-backup-kasten-io.md)

  1. Kubernetes - Wartung 
     * [kubectl drain/uncordon](/kubectl/uncordon-drain.md)
     * [Alte manifeste konvertieren mit convert plugin](/kubectl/convert-plugin.md)

  1. Kubernetes - Tipps & Tricks 
     * [Assigning Pods to Nodes](/tipps-tricks/pods-2-nodes.md) 

  1. Kubernetes - Documentation 
     * [Documentation zu microk8s plugins/addons](https://microk8s.io/docs/addons)
     * [LDAP-Anbindung](https://github.com/apprenda-kismatic/kubernetes-ldap)
     * [Shared Volumes - Welche gibt es ?](https://kubernetes.io/docs/concepts/storage/volumes/)

  1. Linux und Docker Tipps & Tricks allgemein 
     * [Auf ubuntu root-benutzer werden](sudo.md)
     * [IP - Adresse abfragen](ip-a.md)
     * [Hostname setzen](hostname.md)
     * [Proxy für Docker setzen](proxy-docker.md)
     * [vim einrückung für yaml-dateien](/vim/vim-yaml.md)
     * [YAML Linter Online](http://www.yamllint.com/)
     * [Läuft der ssh-server](ssh-running.md)
     * [Basis/Parent - Image erstellen](docker-base-image.md)
     * [Eigenes unsichere Registry-Verwenden. ohne https](insecure-registry.md)
