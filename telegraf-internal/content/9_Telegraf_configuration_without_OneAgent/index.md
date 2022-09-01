## Telegraf configuration without OneAgent

There might be instances that you cannot deploy oneagent on the host to monitor the health of the host or have full-stack monitoring due to limited host units. For those instances, you can configure Telegraf without oneAgent. Please follow  the steps below to configure Telegraf without OneAgent:

1. Within Dynatrace, navigate to Settings > Integration > Dynatrace API and create a token with "Ingest metrics" enabled,
![image](./images/Token.png)

1. Open telegraf.conf file with this comment below:

```
$ sudo nano /etc/telegraf/telegraf.conf
```
3. Uncomment below tags and add the appropriate values in telegraf.conf.

* Uncomment [[outputs.dynatrace]]
* Uncomment api_token = "abcdefjhij1234567890"
* Uncomment url = "https://{your-environment-id}.live.dynatrace.com/api/v2/metrics/ingest"

![image](./images/telegraf_configuration.png)
