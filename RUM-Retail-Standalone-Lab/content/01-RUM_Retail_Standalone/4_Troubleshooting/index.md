## Troubleshooting

In this exercise , we will verify if the OneAgent is monitoring the application.

### Confirming that the RUM JavaScript tag has been correctly injected into the application's HTML

Check for the Dynatrace JavaScript if the application contains the string `ruxitagentjs` in the **Source** files under the `HEAD` element (Click on `script` as well).

```bash
<script> type="text/javascript" src="/ruxitagentjs_[...]_[...].js" data-dtconfig="[...]" </script>
```

![Application-JavaScript-Tag](../../../assets/images/JSTag-Network.png)

### Confirming that the RUM JavaScript tag has downloaded correctly

With the help of browser DevTool, load the application's page and navigate to **Network** section. Confirm if the response of the JavaScript code exists with a response status `200 "OK"`

![Application-JavaScript-Tag](../../../assets/images/JSTag-Source.png)

### Confirming that RUM monitoring data is been sent to Dynatrace

The JavaScript Tag reports back data via XHR or CORS post requests. The requests can be viewed among the **Network** operations using the browser DevTools. The URL related to the request will begin with `rb_` and must have status `200 "OK"` response and must start with `**OK**`

![Application-JavaScript-Tag](../../../assets/images/JSTag-Response.png)

### Exploring Dynatrace

Login to the Dynatrace environment and explore around **Smartscape topology** and under **Host**, select the host. Smartscape helps to identify the internal dependencies - both horizontal and vertical. It is pivotal in identifying the root-cause and help to identify how is your entity/application placed in the environment.
![Smartscape](../../../assets/images/Smartscape.png)


<!-- ------------------------ -->
