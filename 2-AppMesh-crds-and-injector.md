# Installing injectors

App Mesh injector controller, watches for new pods to be created and automatically adds the sidecar data to the pods as they are deployed.

Let's Install Injectors

```
./2_create_injector/create.sh
```

By default, the injector doesn’t act on any pods—you must give it the criteria on what to act on. For the purpose of this workshop, we’ll next configure it to inject the App Mesh sidecar into any new pods created in the prod namespace.

Let's Label namespace to auto inject sidecar

```
kubectl label namespace prod appmesh.k8s.aws/sidecarInjectorWebhook=enabled
```

Make sure injector is running

```
kubectl get pods -nappmesh-inject
```

Install AppMesh Componenets 

```
kubectl apply -f 3_add_crds/mesh-definition.yaml
kubectl apply -f 3_add_crds/virtual-node-definition.yaml
kubectl apply -f 3_add_crds/virtual-service-definition.yaml
```

```
kubectl apply -f 3_add_crds/controller-deployment.yaml
```

Verify the installtion

```
kubectl get pods -nappmesh-system
```


The CRD and injector are AWS-supported open source projects. If you plan to deploy the CRD or injector for production projects, always build them from the latest AWS GitHub repos and deploy them from your own container registry. That way, you stay up-to-date on the latest features and bug fixes.


# Let's move on 

We can move to [3-Configuring-Existing-Microservices.md](3-Configuring-Existing-Microservices.md)
