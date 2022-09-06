## HTTP Monitor
In this section, we will create a HTTP monitor. If we just want to check availability or status of our resources, we can use HTTP monitoring. For example, check availability  of API endpoints.
1. Within Dynatrace-tenant, navigate to **Synthetic**
1. Click on "**Create synthetic monitor** on top right.
1. Click on **Create an HTTP monitor**
1. Click on **Add Http request**
![image](./images/create-http-monitor-1.png)

Further, create the http monitor with the following configuration:
1. Provide "HTTP-monitor" as HTTP monitor name.
1. "HTTP request" as Request type.
1. Provide `http://AWS-IP` as HTTP Request URL - Replace the AWS-IP with your machine IP
1. "ECommerce-app" as Name.
1. Select "GET" as HTTP Method.
1. Click on "Add HTTP requst".
![image](./images/create-http-monitor-2.png)

1. HTTP monitors can be scheduled to run from Dynatrace's private synthetic location to fire HTTP request at the scheduled time. So, configure the HTTP monitor to run every **1 minute** from any **two** available locations.
![image](./images/create-http-monitor-3.png)

1. Review Summary and click on "Create HTTP monitor".

:bulb: We can also create private locations for our private applications or if we just want to test it from our network, please refer to *Dynatrace Help Documents* section (8. Private Synthetic Location).

