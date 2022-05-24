# Uebung runasuser 

## Hinweis:

```
Der USER muss auf dem System nicht existieren.
Die Einstellung 

securityContext:
  runasuser: 12000
  
überschreibt die Einstellung unter welchem User der Docker - Container läuft.
Directive: USER 

Allerdings kommt es zu Problemen, wenn der Docker die Sofware (ENTRYPOINT) und 
nachfolgende Software nicht starten kann und der Container stoppt dann
```

## Example 1: (normal mit root) 

```
# Schritt 1: 
# mkdir runtest 
# cd runtest 
# vi 01-privileged.yml 
apiVersion: v1
kind: Pod
metadata:
  name: ubsi1
spec:  
  containers:
  - name: bb
    image: ubuntu
    command: ["/bin/bash"]
    tty: true
    stdin: true
```

```
# Schritt 2:
# Ausführen 
kubectl apply -f 01-privileged.yml 
kubectl exec -it ubsi1 -- bash 
# id 

```

## Example 2: (als nobody: 65534)

```
# Schritt 1: 
# mkdir runtest 
# cd runtest 
# vi 02-nobody-privileged.yml 
apiVersion: v1
kind: Pod
metadata:
  name: ubsi2
spec:
  securityContext:
    runAsUser: 65534 
  containers:
  - name: bb
    image: ubuntu
    command: ["/bin/bash"]
    tty: true
    stdin: true
```

```
# Schritt 2:
# Ausführen 
kubectl apply -f 01-nobody-privileged.yml 
kubectl exec -it ubsi2 -- bash 
# id 
# touch testfile 
# ls -la 
```

## Example 3: (als 1001 - nutzer existiert nicht) 

```
# Schritt 1: 
# mkdir runtest 
# cd runtest 
# vi 03-user-1001.yml 
apiVersion: v1
kind: Pod
metadata:
  name: ubsi1
spec:
  securityContext:
    runAsUser: 1001  
  containers:
  - name: bb
    image: ubuntu
    command: ["/bin/bash"]
    tty: true
    stdin: true
```

```
# Schritt 2:
# Ausführen 
kubectl apply -f 03-user-1001.yml 
kubectl exec -it ubsi3 -- bash 
# id 
# touch testfile 
# ls -la 
```
