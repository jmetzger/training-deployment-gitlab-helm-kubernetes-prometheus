# Wo spielen Rechte RBAC eine Rolle und wie ? 

## Bereich 1: Welche Objekte darf ich als Helm/kubectl - Nutzer erstellen/bearbeiten/ändern 

```
kubectl 
-> user: 
   -> token: identifizert.

users:
- name: admin
  user:
    token: Q2tJbEsxaUI0eFVDT3haYXJIVGxyYWhsWURHRFlnZ25QWVpNd3lVdi9BST0K

--> über ein Token ->
```

```
Benutzer (User/ServiceAccount) <----->. Rolebinding/Clusterrolebinding   <------> Role/Clusterrole            
```

```
Schritt 1: Authentifizierung -> Du darfst generell erstmal zugreifen...

Schritt 2: Was darfst du ? 
anhand von -> rolebinding/clusterrolebinding -> role/clusterrole  
```



## Welche Berechtigungen ? 

```
1. Ebene 
Welche apiGroups  ? 
z.B. apps/v1 oder alles * 
[''] -> v1 
```

```
2. Ebene 
Welche Ressourcen -> welches Kind 
deployment
* <- für alle 
```

```
3. Ebene -> verbs a.k.a (Operationen) 
list (kubectl get pods) - Liste -- > items 
get (kubectl get pod live-pod) -
create
delete
watch 
update
```


## Bereich 3: Welcher Dienst/Controller/Pod -> darf was machen / abfragen 

```
# Beispiel, was darf ein Pod -> z.B. nginx in Bezug auf Anfragen an den kube-api-server 
# Wenn er bereits im laufenden Betrieb ist. 

# Puzzle - Teil 1: Mit welchem Token / Account greift er zu 
kubectl run nginx --image=nginx 

# Er hängt automatisch den ServiceAccount und das ca-cert und auch den namespace 
kubectl describe po nginx 
kubectl get po nginx -o yaml 

kubectl exec -it nginx -- bash 
# cd /var/run/secrets/kubernetes.io/serviceaccount
# ls -la
# cat namespace 
# cat token 

# So können wir auf den kube-api-server low level zugreifen 
# Es ist ein jwt-token, mit Zusatzinformationen wie ServiceAccount
# Läßt sich mit jwt oder auf jwt.io auslesen 
TOKEN=$(cat token)
# Esi ist immer diese Domain zum Kube-Api-Server 
API_URL=https://kubernetes.default.svc.cluster.local

# Api -> v1 abfragen
# curl --cacert ca.crt -H "Authorization: Bearer $TOKEN" $API_URL/api 
# Alle anderen apis abfragen (welche gibt es) -> z.B. apps/v1 
curl --cacert ca.crt -H "Authorization: Bearer $TOKEN" $API_URL/apis

# Ist mein Token falsch, bekomme ich forbidden 403 
curl --cacert ca.crt -H "Authorization: Bearer $TOKENx" $API_URL/apis 

# Ist mein Token korrekt, aber Pfad falsch auch -> 403 -> apis3 gibt es nicth auf dem Server
curl --cacert ca.crt -H "Authorization: Bearer $TOKEN" $API_URL/apis3 
```

```
Ergebnis des Tokens in jwt.io
eyJhbGciOiJSUzI1NiIsImtpZCI6Ikx1SFlBUlRWNHJ1SlV2T1JxdTdkaElZd0lyOGJzTTVZUmpmM3E2VUJiNm8ifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjIl0sImV4cCI6MTY4NTc5MDc3OSwiaWF0IjoxNjU0MjU0Nzc5LCJpc3MiOiJodHRwczovL2t1YmVybmV0ZXMuZGVmYXVsdC5zdmMiLCJrdWJlcm5ldGVzLmlvIjp7Im5hbWVzcGFjZSI6ImRlZmF1bHQiLCJwb2QiOnsibmFtZSI6Im5naW54IiwidWlkIjoiYzFjZDlhOTItZWQ4Yi00YTc1LTkxYWMtODk3ZWU4NGY1ZTdiIn0sInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJkZWZhdWx0IiwidWlkIjoiZmY2NDJhNjMtNDBiOC00ZDNkLTlkMmEtYzA2MjA2ODdkN2Q0In0sIndhcm5hZnRlciI6MTY1NDI1ODM4Nn0sIm5iZiI6MTY1NDI1NDc3OSwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6ZGVmYXVsdCJ9.xR1qQGOzNu8NLZ8XwsG5ZgIHk-y0Z_pEBWqtKhBGDEFZwbdIlWRbfBN0xHrKqmgib9QvBqosqIM4G2Ozm9Dv-4bEZyjqybq8ylKCmRWVA62LjooS5k7gl1E3ws7qY5G2Jb28MPo-x7Gvgx4MBGCACWsPgfaKLF0kwcbGxHC6VWG7Bgj21di-_aHeuuBslhkGHeSLHyuWOXwOPi7ne59b1rAUKblzmEwNbWqRtGKBjqzelkbAq80GD7-khK3LxbOaB9XVN6LzvvXeqjOXGSVUr3gkE4SNM-R1zYO1raXdD6xJ9cHBbRg_kj3PwpD_dxWDYlG-VU_5Zl6v8SAIlILvrg 

# payload data
{
  "aud": [
    "https://kubernetes.default.svc"
  ],
  "exp": 1685790779,
  "iat": 1654254779,
  "iss": "https://kubernetes.default.svc",
  "kubernetes.io": {
    "namespace": "default",
    "pod": {
      "name": "nginx",
      "uid": "c1cd9a92-ed8b-4a75-91ac-897ee84f5e7b"
    },
    "serviceaccount": {
      "name": "default",
      "uid": "ff642a63-40b8-4d3d-9d2a-c0620687d7d4"
    },
    "warnafter": 1654258386
  },
  "nbf": 1654254779,
  "sub": "system:serviceaccount:default:default"
}
```
