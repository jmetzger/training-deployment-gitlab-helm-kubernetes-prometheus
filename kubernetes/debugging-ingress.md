# Schritt - für - Schritt Debug Ingress 

## 1. Schritt, was sagen die Logs des controller 

```
# -A alle namespaces 
kubectl get pods -A | grep -i ingress 
# jetzt sollten die pods zu sehen
# Dann logs der Pods anschauen und gucken, ob Anfrage kommt 
# Hier steht auch drin, wo sie hin geht (zu welcher PodIP) 
# microk8s -> namespace ingress 
# Frage: HTTP_STATUS_CODE welcher ? z.B. 404 
kubectl logs -n ingress <controller-ingress-pod> 

# FEHLERFALL 1 (in logs):
"Ignoring ingress because of error while validating ingress class" 

# Lösung wir haben vergessen, die IngressClass mit anzugeben 
# Das funktioniert bei microk8s, aber nicht bei Installation aus helm 

# Zeile bei annotations ergänzen 


# kubectl apply -f 03-ingress.yml 

```



## 2. Schritt Pods finden, die als Ingress Controller fungieren 

```
# -A alle namespaces 
kubectl get pods -A | grep -i ingress 
# jetzt sollten die pods zu sehen
# Dann logs der Pods anschauen und gucken, ob Anfrage kommt 
# Hier steht auch drin, wo sie hin geht (zu welcher PodIP) 
# microk8s -> namespace ingress 
# Frage: HTTP_STATUS_CODE welcher ? z.B. 404 
kubectl logs -n ingress <controller-ingress-pod> 
```

## 2. Schritt Pods analyieren, die Anfrage bekommen 

```
# Dann den Pod herausfinden, wo die Anfrage hinging 
# anhand der IP 
kubectl get pods -o wide 

# Den entsprechenden pod abfragen bzgl. der Logs
kubectl logs <pod-name-mit-ziel-ip>

```
