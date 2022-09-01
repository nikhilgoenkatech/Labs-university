## Why use Telegraf with Dynatrace?
In this step, we will enable the 3rd party technology to ingest metrics from Telegraf into Dynatrace.

By adding Dynatrace support to Telegraf, one can now get intelligent observability and automatic root cause analysis into technologies where oneAgent cannot be installed for example, a network component.  The network component is still widely monitored using SNMP, so let us enable telegraf to collect SNMP metrics and with telegraf pushing the metrics to Dynatrace, it will help you to start monitoring the network with Dynatrace.

Before we configure the SNMP, let us quickly look into some basics of SNMP.

