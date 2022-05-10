# Running kubernetes agent in (Auto) DevOps - Mode 

## Steps

```
## Step 1: We take spring

Create New Repository -
name: spring-autodevops-tln1 

o Import Template -> Spring 
o public 

```

```
## Step 2: Enable Auto Devops 

Settings -> CI/CD -> Auto Devops 

# Hier die Varianten zeigen. 

```

```
## Step 3: config für agents anlegen

# .gitlab/agents/gitlab-devops-tn1/config.yaml # Achtung kein .yml wird sonst nicht erkannt. 
# mit folgendem Inhalt 

ci_access:
  projects:
    - id: dummyhoney/spring-autodevops-tln1

```

```
## Step 4: 
# agent registrieren / Cluster connecten 

Infrastruktur > Kubernetes Clusters -> Connect a cluster (Agent) 

Jetzt solltest du den Agent auswählen können und klickt auf Register 
```

```
## Step 5: 
# Du erhältst die Anweisungen zum Installieren und wandelst das ein bisschen ab, 
# für das Training:

# Den token verwendest du aus der Anzeige
# tln1 ersetzt durch jeweils (2x) durch Deine Teilnehmer-Nr. 
helm upgrade --install gitlab-agent-ci-cd-tln1 gitlab/gitlab-agent --namespace gitlab-agent-tln1 --create-namespace --set config.token=<token-from-screen>

```
