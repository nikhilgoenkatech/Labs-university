## Extend Dynatrace with CustomDevice  

### Session Objectives
During the session, we will be creating custom device via using Activegate extension for Python and will attend the following objectives,
1. Installing **Activegate**.
1. Installing **Plugin SDK**.
1. Demo of Activegate plugin.
1. Writing own Activegate extension plugin.
1. Uploading the Plugin in our tenant.

:bulb: As a prerequisites, we will also need Python 3.6 installed in our server. For this session, it is already installed in our respective instances.

Negative
: Use PuTTy (Windows), PowerShell (Windows) or Terminal (Mac), ssh into the instance (IP address using the your PEM Key)

Connect to the EC2 instance using the following credentials:
**Username**: dynatrace
**Password**: dynatrace

Now, gain root access by performing `sudo su`
*Hint: Password for root is also dynatrace*

### Activegate Installation

1. Within Dynatrace tenant, please navigate to **Deploy Dynatrace**

![deploy-dynatrace](../../assets/images/deploy-dynatrace.png)

2. Please scroll down on that screen and click on **Install ActiveGate**

![install-activegate](../../assets/images/install-activegate.png)

3. Click on **Linux**.

![Linux Install](../../assets/images/AG-linux.png)

4. Generate a Paas token by clicking on "Create token", or we can use our previously saved token if we have any.

![PaasToken](../../assets/images/Paastoken1.png)

5. **Copy** the command provided in the "Download the installer using this command on the target host" text field. **Paste** the command into your terminal window and execute it.

![Install2](../../assets/images/Install2.png)
(Optional) Once the download is complete, you can verify the signature by copying the command from the *"Verify signature"* text field, then pasting the command into your terminal window and executing it. Make sure your system is up to date, especially SSL and related certificate libraries.

6. **Copy** the command from "Run the installer with root rights" text field.

![Install3](../../assets/images/Install3.png)

### Plugin SDK Installation

1. Within Dynatrace tenant, please click on **Settings**

![settings](../../assets/images/settings.png)

2. In **Settings > Monitoring > Monitored technologies**, please click on **Add new technology monitoring**

![monitoring](../../assets/images/monitoring.png)

3. Choose **Add ActiveGate extension**

![add-extension](../../assets/images/add-extension.png)

4. Click on **Download the Extension SDK**

![download-sdk](../../assets/images/download-sdk.png)

5. Since the SDK is shipped as a ZIP archive, we will have to extract the files

6. We can use WinSCP, FileZilla or any other program to copy the `.zip` folder to our instance.



7. Please use command below to get into the folder and install the SDK wheel package. To do this you can use pip3. Here is an example, **make sure you use your own version number**:


```
$ cd plugin-sdk-"YOUR VERSION"
$ pip3 install plugin_sdk-"YOUR VERSION"-py3-none-any.whl
$ plugin_sdk
```
We can run `plugin_sdk` to verify the installation was successful

### Demo ActiveGate Plugin
In this step, we will have a look at the demo activeGate extesion before we write our own extension.
Navigate to **activeGateExtension** folder by running
`$ cd /home/ubuntu/extendDynatrace/activeGateExtension`

Under activeGateExtension folder, there are two files that are critical to functioning of an extension. These are as follows:
1. Python file named **demo_activegate_plugin_multi.py**
2. Configuration file named **demo.json**

#### Deep-dive the components:
1. **demo_activegate_plugin_multi.py**
The python file contains the class definition and the methods that are required for working of the extension.
```
class RemoteExamplePlugin(RemoteBasePlugin):

def initialize(self, **kwargs):
self.url = self.config.get("url", "http://127.0.0.1:8976")
self.user = self.config.get("auth_user", "admin")
self.password = self.config.get("auth_password", "admin")
self.alert_interval = self.config.get("alert_interval", 10)
self.event_interval = self.config.get("event_interval", 3)
self.relative_interval = self.config.get("relative_interval", 60)
self.state_interval = self.config.get("state_interval", 60)

self.alert_iterations = 0
self.event_iterations = 0
self.relative_iterations = 0
self.absolute_iterations = 0
self.state_iterations = 0

self.current_entries = 1
self.archived_entries = 0
```

**Query method**
This can be termed as the section that contains the method to collect the data and populate it on Dynatrace. So, broadly query method can be classified into two main parts:
I. **Entity Details**:
The device name/details where the pulled metrics will be reported in Dynatrace
```
group_name = self.get_group_name()
topology_group = self.topology_builder.create_group(group_name, group_name)
topology_group.per_second("service.querries_per_second", self.get_num_querries())
topology_group.report_property(key="group_property", value="group_property_value")
devices = self.get_device_names()
port = 80
for device_name in devices:
topology_device = topology_group.create_device(device_name, device_name)
```

II. **Metrics Endpoint**:
The HTTP endpoint from where the metrics will be fetched and reported to the entity defined in Dynatrace.
```
topology_device.absolute("databases.total_num_entities", self.get_device_entries())
topology_device.relative("databases.replicated_entries", self.get_archived_entries())
for table in self.get_tables_for_device(device_name):
topology_device.absolute("databases.table_size", table_size, {"table_name": table})
table_size = table_size + 100
if self.should_create_event():
topology_device.report_custom_info_event("Custom event!")
topology_device.report_performance_event("Performance problem description", "Performance problem", {})
topology_device.report_property(key="device_property", value="device_property_value")
topology_device.add_endpoint("127.0.0.1", port)
port += 1
```
2. **Configuration file "demo.json"**
The JSON file contains the following components
2.1. **Metadata**
The metadata contains a list of properties used to identify your extension.
2.2 **Metrics**
The metrics section describes the data gathered by the extension. This section provides two metrics that mirror both what our demo application serves, and what the Python code collects.
2.3 **configUI**
ConfigUI and the properties sections define the user interface elements used for extension configuration that will be available on Dynatrace UI.
2.4 **UI**
The UI section will contain details on how the collected metrics would be displayed on UI.


### Build your own extension
In this step, we will build our own extension. As an use-case, we will build an extension to pull host units using Dynatrace-v1 infrastructure API and have it reported on the UI.

#### JSON file
First let us create a new folder which will host our JSON and python file. To do so, run the commands as below:
```
$ cd /home/ubuntu/
$ mkdir my_extension
$ cd my_extension
```
Now let us proceed to write our JSON file say **plugin.json**. The file will contain the properties and metrics that we intend to collect using the extension. Open the file using your favorite editor and define the sections as below:

**The Metadata, Metrics, UI and configUI sections all make up the one JSON file, this file will also be provided at the end of the last section**
1. **Metadata:**
Metadata section in the JSON file would constitute the name of our extension along with other details like className (that would be defined later in the python file) and any python libraries that you will need to install in order for the extension to work.
```
{
"name": "custom.remote.python.hostunits",
"version": "154.5",
"type": "python",
"entity": "CUSTOM_DEVICE",
"metricGroup": "tech.Demo_Technology.HostUnits",
"processTypeNames": ["PYTHON"],
"technologies": ["Custom technology"],
"favicon": "https://lh3.googleusercontent.com/gN6iBKP1b2GTXZZoCxhyXiYIAh8QJ_8xzlhEK6csyDadA4GdkEdIEy9Bc8s5jozt1g=w300",
"source": {
"package": "demo_custom_device",
"className": "DemoCustomDevice",
"install_requires": ["requests>=2.6.0"],
"activation": "Remote"
},
```
2. **Metrics:**
We intend to collect host-units using the extension, so our metrics section would contain a single metrics say **hostUnits**. As the host units are reported as integer in Dynatrace, hostUnits for our extension should be of type *Count*. So, our metrics section would look like below:
```
"metrics": [{
"entity": "CUSTOM_DEVICE",
"timeseries": {
"key": "hostUnits",
"unit": "Count",
"displayname": "Consumed Host Units"
}
}
],
```
3. **UI:**
ui section contains detail of how the metrics should be displayed on Dynatrace UI once collected. In our case, let us display the metrics as *Currently Used Host Units* and have it reported as *line*. So, our ui section would look similar to below:
```
"ui":
{
"keymetrics": [{
"key": "hostUnits",
"aggregation": "COUNT",
"displayname": "Currently Used Host Units"
}
],
"keycharts": [{
"group": "Hosts",
"title": "HostUnits",
"description": "Consumed Host Units",
"explanation": "Consumed Host Units",
"series": [{
"key": "hostUnits",
"displayname": "Host Units",
"color": "#00a6fb",
"seriestype": "line",
"rightaxis": false,
"stacked": false,
"unit": "Count",
"metricexplanation": "Not used"
}
]
}]
}
```

4. **configUI**
The last section configUI would contain details of the configuration that your extension will accept from end-user. In our case, as we are making API calls to Dynatrace, we will need the Dynatrace end-point details - let us call it say **URL**.

Additionally, we will need API-token that will be used to fetch the values - let us call it **apiToken**.Please follow the instructions below to generate Dynatrace API-Token,
1. Within Dynatrace tenant, please navigate to **Access tokens**,

![access-tokens](../../assets/images/access-tokens.png)

2. Click on **Generate new token**,

![generate-new-token](../../assets/images/generate-new-token.png)

3. Please enter a Token name and then search for **Access problem** to find the right API v1 permissions. Click **Generate token**.

![token-one](../../assets/images/token-one.png)

4. We can now copy the token and store it somewhere secure, as this token is generated only once,

![token-two](../../assets/images/token-two.png)

:bulb: With this token, we can now access the Dynatrace API and get all sorts of information about our environment.


So, our configUI would look similar to below:

```
"properties": [{
"key": "url",
"type": "String"
},
{
"key": "apiToken",
"type": "password"
}
],

"configUI": {
"displayName": "Custom Device - HostUnits",
"properties": [
{
"key": "url",
"displayName": "URL",
"displayHint": "https://{your-tenant-id}.dynatracelabs.com"
},
{
"key": "apiToken",
"displayName": "apiToken",
"displayHint": "API Token value that will be used to gather data"
}
]
}
}
```
Save the plugin.json.

[Download plugin.json](https:/images/plugin.json)

We will now move to writing the python file.

#### Python file
Create the python file with class and method definition that will populate and publish the data on our extension. As we have specified DemoCustomDevice as our class name in the source section of metadata, make sure that you define the class as *DemoCustomDevice*.


**API/Endpoint Details**
Before we jump on implementing the method, let us gather our requirements and expore the API (HTTP-endpoint) that will help to pull data. We will leverage 'api/v1/entity/infrastructure/hosts?includeDetails=true' API, so that is the HTTP-endpoint in this use-case.
```
curl -X GET "https://{your-tenant-id}.dynatracelabs.com/api/v1/entity/infrastructure/hosts?includeDetails=true" -H "accept: application/json; charset=utf-8" -H "Authorization: Api-Token {token}"

```

**The Entity Details and Metrics Endpoint section all make the same python file, this completed file will also be provided at the end.**

I. **Entity Details**
Considering the above, our entity details section will have the URL/tenant-details where we can fetch data from (let us initialize it in a variable as *url*) along with API endpoint (let us call it as *dynatraceurl*) along with token required to fetch and publish the data - let us call it *apiToken*. With all this required, our initialize function would look like below:
```
import requests
from ruxit.api.base_plugin import RemoteBasePlugin
import json
class DemoCustomDevice(RemoteBasePlugin):
def initialize(self, **kwargs):
self.url = self.config["url"]
self.apiToken = self.config["apiToken"]
self.dynatraceURL = self.url +                     "/api/v1/entity/infrastructure/hosts?includeDetails=true"
self.headers = {
'Accept': 'application/json; charset=utf-8',
'Authorization':'Api-Token ' + self.apiToken
}
```
II. **Metrics Endpoint:**
Let us define the second method in the python file, say **query** that will contain the logic to pull and publish the data in the extension. In the first part of the method, create a group and device using *create_group* and *create device* library functions. Further, collect the metric and report to dynatrace. So, our method would look like below:
```
def query(self, **kwargs):
# Create group - provide group id used to calculate unique entity id in dynatrace and display name for UI presentation
self.logger.info("URL: %s", self.dynatraceURL)
group = self.topology_builder.create_group(
identifier="CustomDeviceHostUnits", group_name="Custom Device Host Units")
# Create device - provide device id used to calculate unique entity id in dynatrace and display name for UI presentation
device = group.create_device(
identifier="CustomDeviceHostUnits", display_name="Custom Device Host Units")
self.logger.info(
"Topology: group name=%s, device name=%s", group.name, device.name)
# Collect stats
stats = self.getHostUnits("consumedHostUnits")
# report absolute value
device.absolute(key='hostUnits', value=stats)
self.logger.info("HostUnits: %s", stats)
```
Note:
host-units consumed by a host is returned in **"consumedHostUnits"** of the returned JSON object by the API as seen below:
![JSON-object](../../assets/images/json-object-api-call.png)


So, let us gather the values of the `consumedHostUnits` attribute and report in our function as below:
```
def getHostUnits(self, searchkey):
hostinfo = requests.request(
"GET", self.dynatraceURL, headers=self.headers, data={}).content
if hostinfo is not None:
hostUnits = 0
hostdict = json.loads(hostinfo)
hostUnitValues = [a_dict[searchkey] for a_dict in hostdict]
for x in range(len(hostUnitValues)):
self.logger.info(' '.join(map(str, hostUnitValues)))
for val in hostUnitValues:
hostUnits = hostUnits + val
return hostUnits
```
Save the python file.

[Download demo_custom_device.py](https:/images/demo_custom_device.py)

