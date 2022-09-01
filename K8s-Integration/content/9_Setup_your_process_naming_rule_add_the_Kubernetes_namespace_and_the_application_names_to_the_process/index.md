## Setup your process naming rule (add the Kubernetes namespace and the application names to the process)

1. Go to **Settings -> Processes and containers -> Process group naming**
2. Click **Add a new rule**
3. Provide a name say "My-Process-Naming-Rule", then set the Process group name format as **{ProcessGroup:KubernetesNamespace}-{ProcessGroup:NodeJsAppName}**
4. Set the conditions based on Kubernetes namespace and application names (use **Add condition** to add more conditions)
5. Click **Create rule** once we have configured all the fields

> Note: Make sure you have defined the fields as below
![image](./images/K8s-process-naming-rule.png)

