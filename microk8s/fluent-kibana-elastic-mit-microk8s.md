# ElasticSearch / Fluent / Kibana - installieren unter microk8s 

## Installieren 

```
microk8s enable fluentd

# Zum anzeigen von kibana 
kubectl port-forward -n kube-system service/kibana-logging 8181:5601
# in anderer Session Verbindung aufbauen mit ssh und port forwarding 
ssh -L 8181:127.0.0.1:8181 11trainingdo@167.172.184.80

# Im browser 
http://localhost:8181 aufrufen 
```

## Konfigurieren 

```
Discover:
Innerhalb von kibana -> index erstellen 
auch nochmal in Grafiken beschreiben (screenshots von kibana) 
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes

```
