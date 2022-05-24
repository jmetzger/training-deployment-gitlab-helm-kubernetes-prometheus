# Helm - Example - Install bitnami/mysql 

## Prerequisites 

  * kubectl needs to be installed and configured to access cluster
  * Good: helm works as unprivileged user as well - Good for our setup 
  * install helm on ubuntu (client) as root: snap install --classic helm 
    * this installs helm3
  * Please only use: helm3. No server-side comoponents needed (in cluster) 
    * Get away from examples using helm2 (hint: helm init) - uses tiller  

## Example 1: We will setup mysql

```
helm repo add bitnami https://charts.bitnami.com/bitnami 
# paketliste aktualisieren
helm repo update
helm search repo bitnami 
```

```
# download chart - Optional 
# for exercise: to learn how it is structured 
helm pull bitnami/mysql
mkdir lookaround 
cp -a mysql-*.tgz lookaround
cd lookaround
tar xvf mysql-*.tgz 
```

```
helm install my-mysql bitnami/mysql
```


## Example 2 - values in der Kommandozeile 
```
## Vorbereiten - alte Installation löschen
helm uninstall my-mysql 
kubectl delete pvc data-my-mysql-0

# Install with persistentStorage disabled - Setting a specific value 
helm install my-mysql --set primary.persistence.enabled=false bitnami/mysql
helm get values my-mysql 
# Alternative if already installed 

# just as notice 
# helm uninstall my-mysql 

```

## Example 3: values im extra-file (auch mehrere möglich)

```
# Aufräumen 
helm uninstall my-mysql 

```

```
# vi values.yaml
primary:                                                                                             
  persistence:
    enabled: false 
```

```
helm install my-mysql -f values.yaml bitnami/mysql 
# hilfe
helm get values --help 
# Alle, auch defaults anzeigen
helm get values my-mysql --all # alternativ -a  

helm get values my-mysql -o json 
helm get values my-mysql # default: yaml ausgabe 
helm list 
# Allerdings nur 1 Eintrag, bei upgrade sinds mehrere drin 
helm history my-mysql

```

## Referenced

  * https://github.com/bitnami/charts/tree/master/bitnami/mysql/#installing-the-chart
  * https://helm.sh/docs/intro/quickstart/
