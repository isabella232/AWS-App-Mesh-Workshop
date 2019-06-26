# Creating example microservices on Amazon EKS

 These microservices work together to form an app called DJ App, which you use later to demonstrate App Mesh functionality.


```
kubectl apply -f 1_create_the_initial_architecture/1_prod_ns.yaml
```


```
kubectl apply -nprod -f 1_create_the_initial_architecture/1_initial_architecture_deployment.yaml
```


```
kubectl apply -nprod -f 1_create_the_initial_architecture/1_initial_architecture_services.yaml
```

```
kubectl get all -nprod
```

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


