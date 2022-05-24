# Installation gitlab agent (Kubernetes) - using kubectl 

## Steps

```
## Step 1: 

Create New Repository -
name: b-tln<nr> 

With 
README.md

```

```
## Step 2: config für agents anlegen

# .gitlab/agents/gitlab-tln<nr>/config.yaml # Achtung kein .yml wird sonst nicht erkannt. 
# mit folgendem Inhalt 

ci_access:
  projects:
    - id: dummyhoney/b-tln<nr> 

```

```
## Step 3: 
# agent registrieren / Cluster connecten 

Infrastruktur > Kubernetes Clusters -> Connect a cluster (Agent) 

Jetzt solltest du den Agent auswählen können und klickt auf Register 
```

```
## Step 4: 
# Du erhältst die Anweisungen zum Installieren und wandelst das ein bisschen ab, 
# für das Training:

# Den token verwendest du aus der Anzeige
# tln1 ersetzt durch jeweils (2x) durch Deine Teilnehmer-Nr. 
helm upgrade --install gitlab-agent gitlab/gitlab-agent --namespace tln<nr> --create-namespace --set config.token=<token-from-screen>

```
