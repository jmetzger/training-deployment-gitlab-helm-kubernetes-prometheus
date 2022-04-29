# Working with kubectl logs 

## Logs

```
kubectl logs <container>
kubectl logs <deployment>
# e.g. 
# kubectl logs -n namespace8 deploy/nginx
# with timestamp 
kubectl logs --timestamp -n namespace8 deploy/nginx
# continously show output 
kubectl logs -f <container>
```

