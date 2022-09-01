## Runtime Injection:
A deployment strategy you should use when you would want to monitor applications only within a namespace or a selected few. This is partially automatic compared to "App-only" deployment.

# Start the sample-application

We deploy the sample-application in two separate Kubernetes namespaces. Deployment of the application in separate namespaces is for the high availability. Create the namespaces using the below commands:

`kubectl create namespace model-app`
`kubectl create namespace model-app-ha`

To start the sample-application pods, navigate to `/home/ubuntu/k8s` folder.

Under that folder you would be able to view the yaml files for front-end and mongodb deployment. In order to deploy the services and deployment, run the following commands:

`kubectl apply -n model-app -f app-deployment.yaml`
`kubectl apply -n model-app -f mongo-deployment.yaml`
`kubectl apply -n model-app -f mongo-service.yaml`
`kubectl apply -n model-app -f app-service.yaml`

Replicate the deployment of the sample-application in the second namespace with the following commands:

`kubectl apply -n model-app-ha -f app-deployment.yaml`
`kubectl apply -n model-app-ha -f mongo-deployment.yaml`
`kubectl apply -n model-app-ha -f mongo-service.yaml`
`kubectl apply -n model-app-ha -f app-service.yaml`

This would create the deployment, pods and services for the application in both the namespaces.

Now, in order to access the application from outside the k8s cluster, run the below commands:

`kubectl expose deployment/app --type=NodePort --name=model-app -n model-app --port 30005`
`kubectl expose deployment/app --type=NodePort --name=model-app-ha -n model-app-ha --port 30006`

This would create a service which will be accessible from outside the cluster using **< Instance-ip >:30005** and **< Instance-ip >:30006** respectively.

Verify if the services are up and running as expected using commands below. <br>
`kubectl describe service model-app -n model-app`
`kubectl describe service model-app-ha -n model-app-ha`

![image](./images/expose-model-app.png)

### ⚠️ Troubleshooting steps

Negative
: To **check status of pods**, run command below. You should get a **Running** as a return.<br>
`kubectl get pods -n dynatrace`

Negative
: To **check the logs**, run command below.<br>
`kubectl logs -f deployment/dynatrace-oneagent-operator -n dynatrace`

Negative
: To **delete secrets**, run command below. You might have included a wrong secret previously. <br>
`kubectl delete secret --all -n dynatrace`

Negative
: To **delete all pods**, run command below. This will cycle through the pods and you will have new pod instances.<br>
`kubectl delete --all pods -n dynatrace`

Negative
: To **check status of deployments**, run command below. You should get a **Running** as a return.<br>
`kubectl get deployments -n dynatrace`

Negative
: To **check status of services**, run command below. You should get a **Running** as a return.<br>
`kubectl get deployments -n dynatrace`

Negative
: To **delete a service**, run command below.<br>
`kubectl delete service <serviceName> -n dynatrace`

Negative
: Official troubleshooting page could be found [here](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/google-cloud-platform/google-kubernetes-engine/installation-and-operation/full-stack/troubleshoot-oneagent-on-google-kubernetes-engine/)

# Accessing the application

Now, register an user on the application at http://<IP-address>:30005/register or http://<IP-address>:30006/register page with the following details:
**Name**: Guest User
**E-Mail Address**: guestuser@mybank.com
**Password**: GuestUser12@
**Name**: 123456789
![image](./images/register-user-app.png)

As you have completed registering yourself on the application, now login into the app from http://<my-IP>:30005/login or http://<my-IP>:30005/login with the credentials as below:

**E-Mail Address**: guestuser@mybank.com
**Password**: GuestUser12@

# Start the sock-shop application

In this step, let us see how we monitor another separate application that uses the same Kubernetes cluster. We deploy the sock-shop application in a separate namespace. Create the namespace using the below command:

`kubectl create namespace sock-shop`


To start the sock-shop application pods, navigate to `/home/ubuntu/k8s` folder and run the following command:

`kubectl convert -f . | kubectl create -f -`

To **check status of pods** for sock-shop application, run command below. You should get a **Running** as a return.<br>
`kubectl get pods -n sock-shop`


# Setting up Kubernetes Integration

In this step, let us integrate your kubernetes cluster with Dynatrace. The integration would give us a quick view of the environment/pods running on your kubernetes cluster.

In order to integrate kubernetes, you are required to install Environment activeGate.

