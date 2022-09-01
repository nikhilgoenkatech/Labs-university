## Create a Service Account and Cluster role

Create a service account and cluster role for accessing the Kubernetes API. This creates the bearer token necessary to authenticate in the Kubernetes API. Use the following snippet in your shell terminal.

```bash
kubectl apply -f https://www.dynatrace.com/support/help/codefiles/kubernetes/kubernetes-monitoring-service-account.yaml
```

