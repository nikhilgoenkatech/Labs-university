## Defining the Application

In this exercise , we will define the Application detection rule to route the traffic.

As a starting point, all real-user monitoring data is encapsulated in a placeholder application called `My Web Application`. So, unless rules are configured to split/uniquely identify the data for a specific application, all data would continue to be reported back in "My web application" in Dynatrace.

### Application detection rule

Application detection rule would facilitate to create more applications, change existing application mapping, or if needed define more complex rules looking at URL's and not only on domains.

From the navigation menu, select **Settings > Web and mobile monitoring > Applications detection**. Under the **Define application detection rule** section, the list of defined rules are available in the sequential order and the top of list takes the priority over the following rules.

Select **Add detection rule** and select new application and provide a custom name `ECommerce application` for the application under the name text field.

Define which web requests are part of this application by selecting the condition as `If the URL` and scope to be `contains` with the  `AWS IP ADDRESS` and click on **save**.

![Application-Definition](./images/Application-Rule.png)

Now we can access our defined application by navigating to **Application & Microservices > ECommerce application**
![Application URL](./images/application-access.png)

### Application Performance
Dynatrace uses open standard rating for calculating the performance of software application named **Apdex**. Apdex helps to understand the user-experience and is classified based on application-specific thresholds broadly classified as "Satisfactory, Tolerable and Frustrating". These can be modified by navigating to **Applications > Application-name > Edit > General**.
![Apdex](./images/apdex.png)


