# Creating example microservices on Amazon EKS

 These microservices work together to form an app called DJ App, which you use later to demonstrate App Mesh functionality.


First we create a namespace

```
kubectl apply -f 1_create_the_initial_architecture/1_prod_ns.yaml
```

Now we can deploy the initial architecture 

```
kubectl apply -nprod -f 1_create_the_initial_architecture/1_initial_architecture_deployment.yaml
```

And it's services

```
kubectl apply -f 1_create_the_initial_architecture/1_initial_architecture_services.yaml
```


Let's see what we have

```
kubectl get all -nprod
```



And let's see if everything works as expected

```
kubectl get pods -nprod -l app=dj
```

```
kubectl exec -nprod -it <your-dj-pod-name> bash
```

```
curl jazz-v1.prod.svc.cluster.local:9080;echo
```

```
curl metal-v1.prod.svc.cluster.local:9080;echo
```

# Let's move on

We can now continue to [2-AppMesh-crds-and-injector.md](2-AppMesh-crds-and-injector.md)
