# Schritt - für - Schritt Debug Ingress 

## 1. Schritt, Nginx-Pods finden und was sagen die Logs des controller 

```
# -A alle namespaces 
kubectl get pods -A | grep -i ingress 
# jetzt sollten die pods zu sehen
# Dann logs der Pods anschauen und gucken, ob Anfrage kommt 
# Hier steht auch drin, wo sie hin geht (zu welcher PodIP) 
# microk8s -> namespace ingress 
# Frage: HTTP_STATUS_CODE welcher ? z.B. 404 
kubectl logs -n default <controller-ingress-pod> 

# FEHLERFALL 1 (in logs):
"Ignoring ingress because of error while validating ingress class" 

# Lösung wir haben vergessen, die IngressClass mit anzugeben 
# Das funktioniert bei microk8s, aber nicht bei Installation aus helm 

# Zeile bei annotations ergänzen 
annotations:
  kubernetes.io/ingress.class: nginx  
# kubectl apply -f 03-ingress.yml 

```



## 2. Schritt Pods finden, die als Ingress Controller fungieren und log nochmal checken 

```
# -A alle namespaces 
kubectl get pods -A | grep -i ingress 
# jetzt sollten die pods zu sehen
# Dann logs der Pods anschauen und gucken, ob Anfrage kommt 
# Hier steht auch drin, wo sie hin geht (zu welcher PodIP) 
# microk8s -> namespace ingress 
# Frage: HTTP_STATUS_CODE welcher ? z.B. 404 
kubectl logs -n default <controller-ingress-pod> 
```

## 3. Schritt Pods analyieren, die Anfrage bekommen 

```
# Dann den Pod herausfinden, wo die Anfrage hinging 
# anhand der IP 
kubectl get pods -o wide 

# Den entsprechenden pod abfragen bzgl. der Logs
kubectl logs <pod-name-mit-ziel-ip>

```

## Fehlerfall: ingress correct, aber service und pod nicht da 

```
# Es kommt beim Aufrufen der Seite - 503 Server temporarily not available

# Teststellung 
kubectl delete -f 01-apple.yml 

# Seite aufrufen über browser 

# Das sagen die logs 
# Es taucht hier auch keine Ziel-IP des pods auf.
kubectl logs -n default <controller-ingress-pod>
104.248.254.206 - - [22/May/2022:07:23:28 +0000] "GET /apple/ HTTP/1.1" 503 592 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.64 Safari/537.36" 471 0.000 [tln2-apple-service-80] [] - - - - 6c120f60faa57d2ea4409e87d544b1b0 

# Lösung: Hier sollten wir überprüfen, ob 
# a) Der Pod an sich erreichbar ist 
# b) Der service generell erstmal den pod erreichen kann (intern über clusterIP) 

# Wichtig: 
# In den Logs von nginx wird nur eine ip anzeigt, wenn sowohl service als auch pod da sind und erreichbar
# Beispiel: Hier ist er erreichbar !! -> IP 10.224.1.4 
# 10.135.0.5 - - [22/May/2022:07:31:17 +0000] "GET /apple/ HTTP/1.1" 200 12 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.64 Safari/537.36" 497 0.007 [tln2-apple-service-80] [] 10.244.1.4:5678 12 0.004 200 42288726fa35984ccdd07d67aacde8f2
 
```


## Debugging mit Curl 

```
kubectl run -it --rm curly --image=curlimages/curl -- sh 
# alternativ direkt verwenden 
kubectl run -it --rm curly --image=curlimages/curl -- curl 10.14.35.10

# Hiermit dann connection zu services und pods testen 
kubectl get svc pods -o wide  
# damit ips sehen 

```

```
