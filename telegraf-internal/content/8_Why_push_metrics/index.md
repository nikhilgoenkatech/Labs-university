## Why push metrics?
In this step, we will walk through the automatic baseline and uses of pushing telegraf metrics into Dynatrace. We will setup a synthetic browser in order to populate automatic baseline.  Once the metrics are being pushed in Dynatrace, DAVIS AI engine would start digesting the information and identifying the baseline for these metrics. You can setup custom alerts (with auto-detective baseline) so that whenever Dynatrace DAVIS engine deems to be an issue, it will fire an alert.

To do so, we can define a custom event in Dynatrace so that it can start baselining our new metric (*telegraf.snmnp.router.load*) and get an alert when the baseline threshold is breached. Please follow the steps below:

:bulb: For baselining, we will eventually need a week's worth of data to be generated but for this session, we can run it for 20 minutes so that we can test our threshold.

1. We will navigate to **Settings > Anomaly detection > Custom events for alerting**
1. Click on **Create custom event for alerting**
1. Select Category as *telegraf* and select Metric as *telegraf.snmnp.router.load*
1. Select Aggregation as *Average*
![image](./images/Telegraf-custom-event.png)

1. Select 'Monitoring strategy' as **Auto-adaptive baseline**
1. Under *Alert Preview*, we should be able to see data for that metric.
![image](./images/Telegraf-custom-event2.png)

1. We will provide "Router load is high" in the *Title* and select *severity* as "Custom alert".
![image](./images/Event_description.png)

Now, we will change the value for SIMPLE-MIB::simpleInteger and will set a new value for the metric so that it breaches the threshold and generates a problem in Dynatrace. Please go to your instance and run this command below:

```bash
$ snmpset -v 2c -c simple -M+. localhost:5555 SIMPLE-MIB::simpleInteger.0 i 300
```

Within Dynatrace, navigate to **Problems** and we will be able to see a custom problem:
![image](./images/SNMP-problem.png)


