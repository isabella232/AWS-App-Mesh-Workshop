# Prequisites 

1. Make sure you can work with appmesh
```bash
aws appmesh list-meshes
```

you should get 

```
{
"meshes": []
}
```

if you get an erorr: 
```
An error occurred (AccessDeniedException) when calling the ListMeshes operation: User: arn:aws:iam::123abc:user/foo is not authorized to perform: appmesh:ListMeshes on resource: *
```

please make sure you apply the [policy](policies/node.json) to you user


2. Set some environments to make our life easier

```bash
INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep nodegroup)
ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')
echo $ROLE_NAME
```

3. Apply policy to worker nodes

```
aws iam put-role-policy --role-name $ROLE_NAME --policy-name AppMesh-Policy-For-Worker --policy-document file://policies/nodes.json
```

Let's verify that the policy was applied: 

```
aws iam get-role-policy --role-name $ROLE_NAME --policy-name AppMesh-Policy-For-Worker
```


4. Let's test the EKS nodes nad see if they have the required permissions (notice it's configured for Oregon / us-west-2 )

```
kubectl apply -f awscli.yaml
```


and let's check if the policy is attached successfully 
```
kubectl logs jobs/awscli
```

you should see this:
```
{
"meshes": []
}
```

we can delete the job

```
kubectl delete jobs/awscli
```
