## Metrics
In this step, we will identify the metrics in Dynatrace that are being pushed by telegraf.

As we have injected Telegraf via OneAgent, we will be able to see metrics for it. Within your Dynatrace environment, navigate to **Metrics** and search for metrics with "telegraf" prefix in the filter section:
![image](./images/Dynatrace-metric.png)

Please click on details for one of the metrics so see further information:
![image](./images/Metrics.png)

Within Dynatrace, please naviagte to **Explorer Data**, search for *telegraf.snmnp.router.load* (this metric was not nativgely captured by oneAgent but after configuring Telegraf, we are able to see it) and we will be able to create this metric below:
![image](./images/Data-explorer-SNMP.png)

