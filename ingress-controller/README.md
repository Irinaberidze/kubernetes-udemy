## NGINX Ingress Controller

The NGINX ingress controller requires configmap and service account : 

let`s create them in ingress-space namespace. 

```
k create configmap -n ingress-space ingress-config 
 
k create serviceaccount -n ingress-space ingress-serviceaccount
``` 
Service account also requires roles and rolebindings. Make sure those are created as well.


## Creating deployment for ingress controller

```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ingress-controller
  namespace: ingress-space
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      serviceAccountName: ingress-serviceaccount
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
          args:
            - /nginx-ingress-controller
            - --configmap=$(POD_NAMESPACE)/nginx-configuration
            - --default-backend-service=app-space/default-http-backend
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: http
              containerPort: 80
            - name: https
              containerPort: 443
```

## Creating ingress on command line

You can always get help by typing

```
k create ingress -h
```

example :

```
k create ingress my-ingress -n ingress-space --rule="/wear=wear-service:8080" --rule="/watch=video-service:8080"