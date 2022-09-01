## Writing your own extension
Duration: 01:00:00

In this step, we will write our own MongoDB extension.


### List of Learnings
- how to create your own extension
- how to upload it to Dynatrace

### Prerequisites


It is good to start with copying an existing example and changing what is needed.

So, let's grab the `demo_plugin_host` extension and add a little bit to it.

Right now, the file `demo_plugin_host` looks like this:

```python
import requests
from ruxit.api.base_plugin import BasePlugin

class DemoPluginHost(BasePlugin):
def query(self, **kwargs):
stats_url = "http://localhost:8769"
stats = requests.get(stats_url).json()
self.results_builder.absolute(key='battery_level', value=stats['battery_level'])
```

What we are going to do is to get the current connections of our MongoDB database, but you can gather any data you want to monitor with Dynatrace.

First of all, we are going to create a new folder with the name we want, e.g. `custom_mongo_extension`. In there, create one `custom_mongo_extension.py` and a `plugin.json` file. You can copy one of the examples to get started.

So, in this case, we are going to create a python file like this:
**Imports and global variables**
```python
import pymongo
from pymongo.errors import ConnectionFailure
import logging
import ssl
from typing import Optional
from datetime import datetime
import requests

from ruxit.api.base_plugin import BasePlugin
from ruxit.api.selectors import ListenPortSelector

log = logging.getLogger(__name__)

TIMEOUT = 15 * 1000
```

**Class declaration and initzialize-Function**

```python
class CustomMongoExtension(BasePlugin):
def initialize(self, **kwargs):
self.mongodb_client = self.create_client()
```

**Create connection to MongoDB**

```python
def create_client(self) -> Optional[pymongo.MongoClient]:
port = self.config.get("port")
user = self.config.get("auth_user")
password = self.config.get("auth_password")

if not user or not password:
url = f"mongodb://127.0.0.1:{port}/test"
else:
url = f"mongodb://{user}:{password}@127.0.0.1:{port}/test"
auth_db = self.config.get("auth_db", False)
if auth_db:
url = f"{url}/?authSource={auth_db}"

try:
mongodb_client = pymongo.MongoClient(url, ssl=True, ssl_cert_reqs=ssl.CERT_NONE, serverSelectionTimeoutMS=TIMEOUT)
mongodb_client.list_databases()
log.info("Connected to mongodb[SSL]")
return mongodb_client
except ConnectionFailure:
mongodb_client = pymongo.MongoClient(url, serverSelectionTimeoutMS=TIMEOUT)
mongodb_client.list_databases()
log.info("Connected to mongodb")
return mongodb_client
```
**Call function to fetch data from MongoDB**
```python
def query(self, **kwargs) -> None:
if self.mongodb_client is None:
self.mongodb_client = self.create_client()
else:
self.get_server_metrics()
```

**Get Data from MongoDB**
```python
def get_server_metrics(self):
server_status = self.mongodb_client.db.command("serverStatus")
self.send_metric("workshop_current_connections", server_status["connections"]["current"])
```

**Send Data to Dynatrace**
```python
def send_metric(self, key, value):
self.results_builder.absolute(key=key, value=value, entity_selector=ListenPortSelector(self.config.get("port")))
```


The `plugin.json` in your case can look something like this:

**Metadata definition**

```json
{
"version": "3.0.02",
"name": "custom.python.custom_mongo_extension",
"type": "python",
"entity": "PROCESS_GROUP_INSTANCE",
"processTypeNames": ["UNKNOWN", "MONGODB"],
"metricGroup": "tech.MongoDB.CustomExtension",
"source": {
"package": "custom_mongo_extension",
"className": "CustomMongoExtension",
"install_requires": [
"requests>=2.6.0"
],
"activation": "Singleton"
},
```
**Basic chart configuration**
```json
"ui": {
"keyMetrics": [
{
"key": "workshop_current_connections",
"displayname": "Connections",
"mergeaggregation" : "sum"
}
],
"keycharts": [
{
"title" : "Workshop - Current connections",
"group" : "MongoDB metrics",
"series" : [
{
"key" : "workshop_current_connections",
"displayname" : "Connections",
"seriestype" : "bar",
"unit" : "Count"
}
]
}
]
},
```

**Configuration through UI**

```json
"configUI" :{
"displayName": "CustomMongoExtension",
"properties" : [
{ "key" : "auth_user", "displayName": "User", "displayOrder": 1 },
{ "key" : "auth_password", "displayName": "Password", "displayOrder": 2 },
{ "key" : "port", "displayName": "Port", "displayOrder": 3 },
{ "key" : "auth_db", "displayName": "Authentication Database", "displayOrder": 4, "displayHint": "Leave empty for default" }
]
},
```

**Properties that can be used in your extension**
```json
"properties" : [
{
"key" : "port",
"type" :  "String",
"defaultValue" : "27017"
},
{
"key" : "auth_user",
"type" :  "String",
"defaultValue" : "**********"
},
{
"key" : "auth_password",
"type" :  "Password",
"defaultValue" : "**********"
},
{
"key" : "auth_db",
"type": "String",
"defaultValue":  ""
}
],
"metrics": [
{
"timeseries": {
"key": "workshop_current_connections",
"unit": "Count",
"displayname" : "Current connections"
}
}
]
}
```


In the next step, let's take a look at what is new in comparison to the demo we examined earlier.

<!-- ------------------------ -->
