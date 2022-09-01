## Setup your Kubernetes Integration

Go to **Settings -> Cloud and Virtualization -> Kubernetes -> Connect new cluster**

### Get the Kubernetes API URL

Enter the below command and copy it for the **Kubernetes API URL**.

```bash
kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}'
```

### Get the Bearer Token

Enter the below command and copy it for the **Kubernetes Bearer Token**.

```bash
kubectl get secret $(kubectl get sa dynatrace-monitoring -o jsonpath='{.secrets[0].name}' -n dynatrace) -o jsonpath='{.data.token}' -n dynatrace | base64 --decode
```

# Integrate your Kubernetes cluster with Dynatrace

1. Go to **Settings > Cloud and virtualization > Kubernetes.**
2. Select Connect new cluster.
3. Provide a Name say "K8s-cluster", Kubernetes API URL (retrieved from "Get the Kubernetes API URL" step above), and the Bearer token (retrieved from "Get the Bearer   Token" step above) for the Kubernetes cluster.

> Note: Make sure you have enabled the configurables as below:
![image](./images/K8s-cluster.png)

Once successfully connected, click on Kubernetes on the left menu and explore the Kubernetes UI.

![image](./images/K8s-view.png)
![image](./images/K8s-view-detailed-sock-shop.png)
![image](./images/K8s-view-detailed-model-app.png)

Also, there would be some preset dashboards already in your tenant to view your Kubernetes cluster details available at **Dashboards** on the left menu

![image](./images/K8s-preset-dashboard.png)

Kubernetes Cluster overview dashboard is displayed as below. Similarly, we can view the other dashboards as well from the list of **Dashboards**

![image](./images/K8s-preset-dashboard-cluster-overview.png)

# Dynatrace process and service naming rules

Dynatrace automatically detects the processes which are running on the monitored host. With the help of process and service naming rules, application team can customize the names of the monitored processes and services within Dynatrace to suit their requirements by giving meaningful names.

