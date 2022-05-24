# Setup Deployment gitlab.ci.yml

## Schritt 1: manifests - Struktur einrichten

```
# vi manifests/prod/01-pod.yml 

apiVersion: v1
kind: Pod
metadata:
  name: nginx-static-web2
  labels:
    webserver: nginx
spec:
  containers:
  - name: web
    image: bitnami/nginx

```

## Schritt 2: gitlab-ci.yml mit kubectl apply --recursive -f 

```
# CI-CD -> Editor oder .gitlab-ci.yml im Wurzelverzeichnis 
# only change in stage: build 
image: 
  name: bitnami/kubectl
  entrypoint: [""]

deploy:
  stage: deploy
  script:
    - set
    - kubectl config get-contexts
    - kubectl config use-context dummyhoney/b-tln1:gitlab-tln1
    - kubectl config set-context --current --namespace tln1
    - ls -la
    - kubectl apply --recursive -f manifests/prod

```

## Schritt 3: pipeline anschauen

  * War es erfolgreich - kein Fehler ? 

## Schritt 4: Sichtprüfen mit kubectl über Client (lokaler Rechner/Desktop) 

```
kubectl get pods | grep web2 
```
