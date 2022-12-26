## Creating volume to store logs in a pod definition file.

Configuring a volume to store the logs at /var/log/webapp on the host.


* Name: webapp

* Image Name: kodekloud/event-simulator

* Volume HostPath: /var/log/webapp

* Volume Mount: /log


```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: webapp
  name: webapp
spec:
  containers:
  - image: kodekloud/event-simulator
    name: webapp
    volumeMounts:
    - mountPath: /log
      name: log-volume
  volumes:
  - name: log-volume
    hostPath:
      # directory location on host
      path: /var/log/webapp
      # this field is optional
      type: Directory
```


## Creating persistent Volume

Below creates persistent volume with specifications:


* Volume Name: pv-log

* Storage: 100Mi

* Access Modes: ReadWriteMany

* Host Path: /pv/log

* Reclaim Policy: Retain

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
spec:
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /pv/log
```

to check run:
 ```
  k get persistentvolume
 ```

## Creating Persistent Volume Claim

Create a Persistent Volume Claim with the given specification.


* Volume Name: claim-log-1

* Storage Request: 50Mi

* Access Modes: ReadWriteOnce


```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 50Mi
```
to check run:
```
k get persistentvolumeclaim
```