## Pre-requisites
Within your tenant, navigate to Deploy Dynatrace > Kubernetes
![image](../../../assets/images/01-Dynatrace-hub-Kubernetes.png)

Further, click on "Monitor Kubernetes".

Now, from the drop-down, select the following config values:
Now, click on  for "PaaS Token", select "Create a new PaaS token" and click to open in a new window. Once navigated to the page, click on "Generate Token" to create a token.
![image](../../../assets/images/01-Dynatrace-hub-Kubernetes-02.png)

Give an appropriate name say "PaaS-k8s" and copy the token value in a temporary file.

![image](../../../assets/images/01-PaaS-token.png)

Similarly, for "API Token" select "Create a new API token" and click to open in a new window.

![image](../../../assets/images/01-API-token.png)

*NOTE*:
When selecting the permissions, make sure you have the Access problem and event feed, metrics, and topology under "API v1" setting enabled for the API token.

<!-- ------------------------ -->
