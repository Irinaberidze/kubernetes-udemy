## Security context in Kubernetes

to configure security context on the container add below under spec:

```
apiVersion: v1
kind: Pod 
metadata:
  name: ubuntu-sleeper-2
spec:

# to modify security context add below in spec:

  securityContext:
    runAsUser: 1000

  containers:
  - name: ubuntu
    image: ubuntu
    command:  
      - "sleep"
      - "3600"
```

this command runs container as user 1000.

to set the same configuration on the container level move the whole section under container field and add capabilities:

```
apiVersion: v1
kind: Pod 
metadata:
  name: ubuntu-sleeper-2
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command:  
      - "sleep"
      - "3600"

# moved under container and added capabilities on container level

    securityContext:
      runAsUser: 1000
      capabilities:
        add: ["MAC_ADMIN"]
```



