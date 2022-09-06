## Oneagent Operator Installation
In this step, we will install the oneagent operator.

Connect to EC2 instance with the following credentials:
**Username**: dynatrace
**Password**: dynatrace
Further, execute `sudo su` to get access to root priviledge.

Dynatrace support multiple deployment strategies to install oneAgent Operator. We will use Helm approach.
Within EC2 instance, use the following command to install Helm 3.
`sudo snap install helm --classic`

1. Once helm is installed, add Dynatrace OneAgent Helm repository as below:
```bash
helm repo add dynatrace https://raw.githubusercontent.com/Dynatrace/helm-charts/master/repos/stable
```

2. Create a namespace *dynatrace* which will hold the operator deployment and it's dependencies. To create the namespace, use the command as below:
```bash
kubectl create namespace dynatrace
```

3. Now, create a values.yml with the text as below:
```bash
platform: "kubernetes"
operator:
image: ""
oneagent:
name: "oneagent"
apiUrl: "https://ENVIRONMENTID.live.dynatrace.com/api"
image: ""
args:
- --set-app-log-content-access=true
env: {}
nodeSelector: {}
labels: {}
skipCertCheck: false
disableAgentUpdate: false
enableIstio: false
dnsPolicy: ""
resources: {}
waitReadySeconds: null
priorityClassName: ""
serviceAccountName: ""
proxy: ""
trustedCAs: ""
secret:
apiToken: "DYNATRACE_API_TOKEN"
paasToken: "PLATFORM_AS_A_SERVICE_TOKEN"

```

Replace the apiToken and paasToken configurables with the values retrieved earlier and set apiURL to point to your tenant.

For SaaS, configure apiUrl as https://ENVIRONMENTID.live.dynatrace.com/api, where ENVIRONMENTID is your tenant-id
For Managed cluster, use apiUrl as https://my-server/ENVIRONMENTID/api, where my-server is the domain name/ip of your server and ENVIRONMENTID is your managed environment. Once replaced, save and exit the file.

4. Lastly, run the below command to apply the YAML

```bash
$ helm install dynatrace-oneagent-operator dynatrace/dynatrace-oneagent-operator -n dynatrace --values values.yaml
```

Once succesful, you will get a prompt similar to the below and you will see the host appearing when you click on **Show Deployment status**

```bash
NAME: dynatrace-oneagent-operator
LAST DEPLOYED: Wed Mar 03 02:01:30 2021
NAMESPACE: dynatrace
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing dynatrace-oneagent-operator.

Your release is named dynatrace-oneagent-operator.
```

<!-- ------------------------ -->

