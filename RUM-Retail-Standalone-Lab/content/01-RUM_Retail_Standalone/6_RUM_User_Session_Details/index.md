## RUM User Session Details
A **user action** is an interaction with the web browser on an application that involves a/multiple calls to the web server that may/may-not potentially have multiple nested calls and a **user session** contains those set of user-actions.

In this exercise , we will be highlighting "User Sessions" and different user actions captured in the user-session. We will also be looking into the waterfall model to view application load times.

1. Create a user session by navigating through the sample application in a browser window.
![GIF](./images/Usersession.gif)

1. Navigate back to the Dynatrace tenant and view the user action
![US3](./images/US3.png)

1. Navigate to session details for further drill-down
![US4](./images/US4.png)

For more details on how dynatrace defines an user-session, please refer to *Dynatrace Help Documents* section (4. User Session).


### Waterfall Analysis
Dynatrace captures user experience and performance data by monitoring individual user actions. Typically a user action begins with a click on an HTML control (for example, a button or link). The browser then loads the requested data, either by navigating to a new page or via an XHR/fetch call. JavaScript callbacks are then executed, the DOM tree is built or changed, and the web application is then once again ready for a new user action.

Select a user action - for example, in this case we will select **Loading of page /cart/** and perform a waterfall analysis. Select a user action - in this case we will select **Loading of page /cart/** and Click on **perform a waterfall analysis**

![US5](./images/US5.png)
![US6](./images/US6.png)

Positive
: Waterfall model can be extremely useful to debug/analyse a specific request that may have occurred  as a result of the user-action and trace it back to the backend call.
In the below example, we used waterfall analysis to analyse the error that occurred  while downloading a specific image in the application
![ImageError](./images/ImageError.gif)


### Verify real user identifer is equal to cookie value
Dynatrace creates multiple cookies in order to uniquely identify a user-session. One of such cookies that Dynatrace create on end-user's system is **rxVisitor** and is pivotal in identifying the user uniquely.

In order to verify the cookie information, navigate to **User Sessions** within your tenant and locate the user-session. Under the user-session screen, you will be able to identify the unique identifier as "Real User Identifier".
![cookie value 2](./images/cookie_value_2.png)

Navigate back to the application within your browser and using chrome dev tools, find the cookie value and verify it matches with the real user identifier.

![cookie value 1](./images/cookie_value_1.png)


Positive
: In the following sessions, we will be taking the next steps to improve the RUM data to suit different users like business users, developers that would help to analyse/debug/improve the performance for application's end-users.

