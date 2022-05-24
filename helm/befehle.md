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
# history anzeigen 
helm history my-mysql 

# Release installieren - my-mysql ist hier hier release-name 
helm install my-mysql bitnami-mysql 
helm install [name] [chart] --dry-run --debug -f <your_values_file> # dry run 
# + verwendete values anzeigen 
helm get values 

# upgrade, wenn vorhanden, ansonsten install 
helm upgrade --install my-mysql bitnami/mysql 

# Nur template parsen - ohne an den kube-api-server zu schicken  
helm template my-mysql bitnami/mysql > test.yml 
# template und hilfeseite aufgaben und vorher alles an den kube-api-server 
# zur Validierung schicken
helm install --dry-run my-mysql bitnami/mysql 


```
