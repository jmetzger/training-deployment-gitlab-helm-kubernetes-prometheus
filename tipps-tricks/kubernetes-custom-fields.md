# kubectl get nodes custom fields 


```
kubectl get nodes  -o=custom-columns=NAME:.metadata.name,RACK:.metadata.labels.rack
NAME                   RACK
pool-tg5g9rh4y-cw8mb   1
pool-tg5g9rh4y-cw8mr   1
pool-tg5g9rh4y-cw8mw   2
```
