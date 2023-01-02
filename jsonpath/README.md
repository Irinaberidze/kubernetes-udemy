# example:
Use a JSON PATH query to identify the context configured for the aws-user in the my-kube-config context file and store the result in /opt/outputs/aws-context-name

solution

```
k config view --kubeconfig=my-kube-config -o=jsonpath='{.contexts[?( @.context.user == "aws-user" )].name}' > /opt/outputs/aws-context-name
```

