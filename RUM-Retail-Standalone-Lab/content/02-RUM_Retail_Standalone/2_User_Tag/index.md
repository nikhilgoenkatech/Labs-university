## User Tag
In this exercise, we will configure User-tagging rules so that dynatrace can identify the user sessions with unique names closely assigned with your application behaviour.

Dynatrace provides multiple avenues to define the user-tag, we will use CSS selector for this exercise. Please follow the steps below:
1. Within your application page, select the string starting with Welcome on top of your screen as seen in the screenshot.
1. Right click and select *Inspect* from the options.
![usertag1](./images/Usertag-1.png)
1. This would open Element tab in the developer tools, right-click on the element and select *Copy* option followed by *Copy selector*
![usertag2](./images/Usertag-2.png)
1. Once you have identified the CSS selector , within your tenant navigate to application configuration page by following **Frontend > ECommerce application > Edit**
1. Select **Capturing** and click on **User tag**
1. Select **Add user tag rule**
1. Now moving to the application configuration page, Select *CSS selector* and paste the value of CSS selector copied earlier as `#topNavBar > ul:nth-child(2) > p`.
1. This would help dynatrace to retrieve the value of configured CSS selector. However, the CSS element has Welcome string prefixed to the username, so, to slice only the username, we will use regex functionality and filter the "Welcome" string. To do so, enable "Apply cleanup rule" and define the regex as **welcome(.*+)**
1. Click on **Add user tag**, followed by *Save changes*.
![usertag3](./images/usertag6.png)

Once you have applied the user-tag configuration, generate a session (need to login again to capture the new user tag), navigate to *User sessions* screen and you would notice Dynatrace is identifying your session with the correct Username (instead of the Anonymous user with rxVisitor ID).
![usertag4](./images/usertag4.png)

### Key User Actions
Whilst Dynatrace monitors and reports on all user-actions, there are some user-actions that are more critical to business like *payment, add to cart, checkout, etc.*. So for such use-cases, Dynatrace provides a feature to mark these user-actions as "Key User actions". Marking a request as *key user action* feature, enables you to achieve the following:
1. Customize apdex thresholds for these user actions
2. Create a SLO/SLA tile for the key-user actions
3. Allow you to report/create a dedicated dashboard tile (using custom chart)

In order to mark your request as key-request, follow the steps as below:
1. Navigate to the applications page and the select the application
2. Click view full details under the Top 3 user actions
3. Under Top 100 user actions, select a user action
4. On the top-right corner of the User action details page, select Mark as key user action. The selected user action will now be displayed under Key user actions on the User action analysis page

![KeyUserAction](./images/KeyUserAction.png)

It will reflect as below under user actionpage from application,
![KeyUserAction1](./images/keyUserActionCaptured.png)

