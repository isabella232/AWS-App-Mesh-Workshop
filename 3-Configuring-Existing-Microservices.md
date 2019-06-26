# Configuring existing microservices

The mesh component serves as the App Mesh foundation, and must be created first. We’ll call our mesh dj-app, and define it in the prod namespace

```
kubectl create -f 4_create_initial_mesh_components/mesh.yaml
```


```
kubectl get meshes -nprod
```


```
aws appmesh list-meshes
```


```
aws appmesh describe-mesh --mesh-name dj-app
```

All services (physical or virtual) that will interact in any way with each other in App Mesh must first be defined as Virtual Node objects. Abstracting out services as Virtual Nodes helps App Mesh build rulesets around inter-service communication. In addition, as we define Virtual Service objects, Virtual Nodes are referenced as the ingress and target endpoints for those Virtual Services. Because of this, it makes sense to define the Virtual Nodes first.

```
kubectl create -nprod -f 4_create_initial_mesh_components/nodes_representing_physical_services.yaml
```

```
cat 4_create_initial_mesh_components/nodes_representing_physical_services.yaml
```


```
kubectl get virtualnodes -nprod
```

Create virtual services

```
kubectl apply -nprod -f 4_create_initial_mesh_components/virtual-services.yaml
```


If you inspect the YAML, you may notice that it created two virtual service resources. Requests made to jazz.prod.svc.cluster.local are intercepted by App Mesh and routed to the virtual node jazz-v1.
Similarly, requests made to metal.prod.svc.cluster.local are routed to the virtual node metal-v1

Remember to use fully qualified DNS names for the virtual service’s metadata.name field to prevent the chance of name collisions when using App Mesh cross-cluster.

```
cat 4_create_initial_mesh_components/virtual-services.yaml
```

```
kubectl get svc -nprod
```


To provide the jazz and metal virtual services with resolvable IP addresses and hostnames, define them as Kubernetes services that do not map to any deployments or pods. Do this by creating them as k8s services without defining selectors for them. Because App Mesh is intercepting and routing requests made for them

```
kubectl create -nprod -f 4_create_initial_mesh_components/metal_and_jazz_placeholder_services.yaml
```

```
kubectl get svc -nprod
```


Because the dj pods were already running before the injector was created, you’ll now force them to be re-created, this time with the sidecars auto-injected into them.

```
{
kubectl patch deployment dj -nprod -p "{\"spec\":{\"template\":{\"metadata\":{\"labels\":{\"date\":\"`date +'%s'`\"}}}}}"
kubectl patch deployment metal-v1 -nprod -p "{\"spec\":{\"template\":{\"metadata\":{\"labels\":{\"date\":\"`date +'%s'`\"}}}}}"
kubectl patch deployment jazz-v1 -nprod -p "{\"spec\":{\"template\":{\"metadata\":{\"labels\":{\"date\":\"`date +'%s'`\"}}}}}"
}
```


```
kubectl get svc -nprod
```

You can now see that you have 2 contianers running
```
kubectl describe pods/<dj-pod> -nprod
```


## Test App Mesh


```
kubectl get pods -nprod -lapp=dj
```


```
kubectl exec -nprod -it <your-dj-pod-name> bash
```

```
curl jazz.prod.svc.cluster.local:9080;echo

curl metal.prod.svc.cluster.local:9080;echo
```

## Great Job! Let's move on 

We can now continue to [4-Canary-testing.md](4-Canary-testing.md)
