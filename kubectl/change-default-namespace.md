# Default Namespace von kubectl ändern 

## How ? 

```
kubectl config set-context --current --namespace=<insert-namespace-name-here>
# Validate it
kubectl config view --minify | grep namespace:
```

## Reference:

  * https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
