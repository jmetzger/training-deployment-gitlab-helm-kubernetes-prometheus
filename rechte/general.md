# Wo spielen Rechte RBAC eine Rolle und wie ? 

## Bereich 1: Welche Objekte darf ich als Helm/kubectl - Nutzer für Objekte erstellen/bearbeiten/ändern 

```
kubectl 
-> user: 
   -> token: identifizert.

users:
- name: admin
  user:
    token: Q2tJbEsxaUI0eFVDT3haYXJIVGxyYWhsWURHRFlnZ25QWVpNd3lVdi9BST0K

--> über ein Token ->
```

```
Benutzer (User/ServiceAccount) <----->. Rolebinding/Clusterrolebinding   <------> Role/Clusterrole            
```

```
Schritt 1: Authentifizierung -> Du darfst generell erstmal zugreifen...

Schritt 2: Was darfst du ? 
anhand von -> rolebinding/clusterrolebinding -> role/clusterrole  
```



## Welches Berechtigungen ? 

```
1. Ebene 
Welche apiGroups  ? 
z.B. apps/v1 oder alles * 
[''] -> v1 
```

```
2. Ebene 
Welche Ressourcen -> welches Kind 
deployment
* <- für alle 
```

```
3. Ebene -> verbs a.k.a (Operationen) 
list (kubectl get pods) - Liste -- > items 
get (kubectl get pod live-pod) -
create
delete
watch 
update
```
