# Public IP auslesen

## Hinweis 

```
Get nicht immer, muss in nodes objekt eingetragen sein vom Kube-Api-Server
```

## Examples 

```
kubectl get nodes -o yaml 
kubectl get nodes -o yaml  | grep -i address
kubectl get nodes -o jsonpath='{.items[0].status.addresses[2]}' 
kubectl get nodes -o jsonpath='{.items[0].status.addresses[2].address}‘

```
