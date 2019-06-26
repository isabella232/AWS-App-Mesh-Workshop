# Canary testing with v2

A canary release is a method of slowly exposing a new version of software. The theory behind it is that by serving the new version of the software initially to say, 5% of requests, if there is a problem, the problem only impacts a very small percentage of users before its discovered and rolled back.

So now back to our DJ App scenario… the V2 of the metal and jazz services are out, and they now include the city each artist is from in the response. Let’s see how we can release v2 versions of metal and jazz services in a canary fashion using App Mesh.

When we’re complete, requests to metal and jazz will be distributed in a weighted fashion to both the v1 and v2 versions.

Let's deploy a new version of our microservices

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


# Congrats on implementing the DJ App onto App Mesh!

Let's look at another aspect of Service Mesh [5-Observability.md](5-Observability.md)
