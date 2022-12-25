# creating secret for docker registry:

to get the command run:

```
k create secret docker-registry -h
```
find the command like below and modify it;

```
kubectl create secret docker-registry private-reg-cred --docker-username=dock_user --docker-password=dock_password --docker-server=myprivateregistry.com:5000 --docker-email=dock_user@myprivateregistry.com 
```

after creating secret check secrets:

k get secret

finally we need to inject secret to our pod yaml file by adding
imagePullSecrets section. check my-deployment.yaml file for the example.
also make use of kubernetes official documentation by searching as pull image secret.

