## Setting up Dynatrace on Kubernetes
In this step, install the Dynatrace Operator on your Kubernetes cluster so that the OneAgent can report and collect metrics from different pods.

Use PuTTy (Windows), PowerShell (Windows) or Terminal (Mac), ssh into the instance using the following credentials:
**Username**: `d1prumworkshop`
**Password**:  `dynatrace`

Alternatively, you can connect to the SSH terminal by clicking on the icon as below:
![image-ssh-terminal](../assets/images/ssh_terminal_access.png)

Further become root user by executing the below command:
```
$ sudo su
```
Note: Password for root is **dynatrace**

Let us install the Dynatrace Operator on your Kubernetes cluster so that the OneAgent can report and collect metrics from different pods. Dynatrace support multiple deployment strategies to install the Dynatrace Operator, for Kubernetes integration. In this tutorial, we will use the automated mode.

```bash
$ kubectl config view --raw > /home/ubuntu/.kube/config

$ kubectl config view --raw > ~/.kube/config

$ export CLUSTER_SERVER=$(microk8s config | grep "server:" | sed 's/^.*server: //')

$ kubectl config set-cluster microk8s-cluster --insecure-skip-tls-verify=true --server="$CLUSTER_SERVER"

```

Negative
:If you come across the following error
![image](../assets/images/kube.png)

Please execute the following command to resolve the error
```bash
$ cd $HOME

$ mkdir .kube

$ cd .kube

$ microk8s config > config

$ export CLUSTER_SERVER=$(microk8s config | grep "server:" | sed 's/^.*server: //')

$ kubectl config set-cluster microk8s-cluster --insecure-skip-tls-verify=true --server="$CLUSTER_SERVER"

```
# Deployment strategies
Dynatrace supports mutiple deployment strategies for monitoring kubernetes. During the step, we will go through the different strategies and the benefits.

