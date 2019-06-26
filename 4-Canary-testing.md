# Canary testing with v2



Deploy the jazzv2 deployment

```
kubectl apply -nprod -f 5_canary/jazz_v2.yaml
```


You can see that the traffic weight is 100 towards jazzv1

```
kubectl describe virtualservice jazz -nprod
```



Let's change that

```
kubectl apply -nprod -f 5_canary/jazz_service_update.yaml
```

You can see that the traffic weight is split between the 2 versions

```
kubectl describe virtualservice jazz -nprod
```

Let's do something similar for the metal microservice


```
kubectl apply -nprod -f 5_canary/metal_v2.yaml
```

And let's split the traffic evenly between the 2 versions:

```
kubectl apply -nprod -f 5_canary/metal_service_update.yaml
```

```
kubectl describe virtualservice metal -nprod
```


### Let's test everything 

```
kubectl exec -nprod -it `kubectl get pods -nprod -l app=dj` bash

while [ 1 ]; do curl http://metal.prod.svc.cluster.local:9080/;echo; done

Ctrl + C

while [ 1 ]; do curl http://jazz.prod.svc.cluster.local:9080/;echo; done
```



