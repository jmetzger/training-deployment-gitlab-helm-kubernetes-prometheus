# Auto Devops Domain Variablen setzen. 

## Warum ? 

  * Damit das Auto Deployment funktioniert, muss ein KUBE_INGRESS_BASE_DOMAIN eingerichtet.

```
Ist die eingetragen wildcard - domain in der DNS bspw. *.lab1.t3isp.de, wäre die Base - Domain lab1.t3isp.de
```

## Wo setzen ? 

  * Wir können dies bspw. bei den CI/CD - Variablen setzen 
  * Für das Training o.k. aber für Betrieb, würde ich das eher in:
    * Menu > Admin > Settings > CI/CD > Continuous Integration and Delivery
    * oder: aber für die Gruppe setzen.

```
Settings -> CI/CD -> Variables:
# Example 
KUBE_INGRESS_BASE_DOMAIN lab1.tisp.de 
Uncheck -> Protected 

```
