
## CloudWatch logs with Fluentd

Make sure you still have the Environment Variable $ROLE_NAME configured from 0-Prequisites.md

Let's add permission to write logs to cloudwatch
```
aws iam put-role-policy \
      --role-name $ROLE_NAME \
      --policy-name Worker-Logs-Policy \
      --policy-document file://6_observability/policy.json
```


```
kubectl -n kube-system get po -l=k8s-app=fluentd-cloudwatch
# validate that the Fluentd pods are up and running:
$ kubectl -n kube-system get po -l=k8s-app=fluentd-cloudwatch
NAME                       READY   STATUS    RESTARTS   AGE
fluentd-cloudwatch-7ls6g   1/1     Running   0          13m
fluentd-cloudwatch-mdf9z   1/1     Running   0          13m
```

Open AppMesh Console 

Choose a node and edit it

Choose "Additional configuration"

In Logging change the output to :

```
/dev/stdout
```

Now you can go to Cloudwatch logs and see the logs


\o/


## X-Ray

[See walkthrough here](https://github.com/aws/aws-app-mesh-examples/blob/master/walkthroughs/eks/o11y-xray.md)
