# Helm - Wichtige Befehle 

```
# Repos
helm repo add gitlab http://charts.gitlab.io 
helm repo list 
helm repo remove gitlab 
helm repo update 

# Suchen 
helm search repo mysql # in allen konfigurierten Repos suchen 

# Chart herunterladen 
helm repo pull bitnami/mysql 

# Releases anzeigen 
helm list 

# Release installieren - my-mysql ist hier hier release-name 
helm install my-mysql bitnami-mysql 

# Nur template parsen 
helm template bitnami/mysql > test.yml 



```
