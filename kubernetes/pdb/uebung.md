# Übung PDB (Pod Disruption Budget) 

## Warum ? 

```
PDB ermöglicht, dass sichergestellt wird, dass immer 
eine bestimmte Mindestanzahl an Pods für ein bestimmtes Label laufen
-> minAvailabe 

(oder max eine maximale Anzahl nicht verfügbar: maxUnavailable

```

## Wann ? 

```
Das ganze funktioniert nur fÜr:
Voluntary disruptions

(D.h. ich beeende bewusst durch meine Aktion einen Pod, z.B. durch drain'en) 

Für 
Involuntary disruptions
.. z.B. Absturz.. funktioniert das ganze nicht 
```

## Übungsbeispiele (nur für Gruppen, wo jeder sein eigenes Cluster hat) 

```
# Situation: 3-node-cluster 
```

```
# Schritt 1: 
# Deployment erstellen
# mkdir pdb-test 
# cd pdb-test
# vi 01-pdb-test-deploy.yml 
# vi nginx-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-test-deployment
# tells deployment to run 2 pods matching the template
spec:
  selector:
    matchLabels:
      app-test: nginx
  replicas: 10
  template:
    metadata:
      labels:
        app-test: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```


```
# Schritt 2:
kubectl apply -f 01-pdb-test-deploy.yml 

```

```
# Schritt 3: 
# vi 02-pdb-test-budget.yml 
# pdb festlegen 
# % oder Zahl möglich
# auch maxUnavailable ist möglich statt minAvailable 
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: pdb-test
spec:
  minAvailable: 50%
  selector:
    matchLabels:
      app-test: nginx
```

```
# Schritt 4: pdb apply'en 
kubectl apply -f 02-pdb-test-budget.yml
```


```
# Schritt 5: Erste node drainen
# hier geht noch alles 
kubectl drain <node1>
kubectl get pdb 
kubectl describe pdb 
```


```
# Schritt 6: 2. Node drainen 
# hier geht auch noch alles, aber evtl. bereits meldungen
# von System-Pods 
kubectl drain <node2>
kubectl get pdb 
kubectl describe pdb
```

```
# Schritt 7: 3. Node drainen 
# jetzt kommen meldungen - pod cannot be evicted 
# von System-Pods 
kubectl drain <node3>
kubectl get pdb 
kubectl describe pdb
```
