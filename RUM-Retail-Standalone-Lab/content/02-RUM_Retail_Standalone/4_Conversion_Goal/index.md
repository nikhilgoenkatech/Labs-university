## Conversion Goal

In this exercise, we will define Conversion goal to monitor the user session which has met the goal defined.

Conversion goals can be defined for reaching specific user actions or destination URLs and also for session information (for example, session duration longer than 5 minutes or sessions with more than 10 user actions. For this example, a user would need to complete at least 10 user actions in a single session to reach this conversion goal).

### Creating Conversion Goal
One of the important goal for an e-commerce application would be to identify users who have proceeded to checkout. However, in our application we have multiple checkout requests or user-actions like /checkout/payment/, /checkout/success/, /checkout/ that would constitute the checkout process. So, for such a use-case we would first apply user-action naming rule and then mark the Useraction as "Conversion Goal". To do so, follow the steps as below:
1. Navigate to application settings by selecting the application under Applications page, followed by **Browse [...] > Edit** and selecting **User actions** under *Capturing*.
1. Under *Add naming rule* for load actions configuration, input `Payment-gateway` for Naming Pattern.
1. Click on **Add condition**.
1. Select "pageURL(default)" as Input, Operator as contains and input text `/checkout/`, click on *save*.
This would mean all the user-URLs that contain checkout would be renamed to "Payment-gateway" instead of the actual request (as defined on your application).
![User-action-renaming](./images/Useraction-renaming.png)

Now, as we have renamed all the loading /checkout/ requests as "Payment-gateway", let us create a conversion goal for Payment-gateway.

To define the conversion goal, navigate to the application settings by clicking on **Browse [...] > Edit**. Select **Session Replay and behaviour > Conversion goals** and click **Add goal** to set up the goal.
1. Start with defining the **Name** of the goal as `Payment Initiated`
1. Select respective **Type of goal** as `User Action`
1. followed by **Rule applies to:** to be `Load actions`
1. Define the **Rule:** to be `Action name > contains > Payment-gateway`
1. Click on the **Add goal** to save the defined conversion goal.
![Conversion-Goal](./images/Goal.png)

### Measuring Conversion Goal
1. Generate a user session to initiate a payment in the application.
1. Navigating to the **User session**.
1. Use the filter option to sort the user session which met the conversion goal defined by selecting **Conversion Goal**
1. Selecting the **Goal**
1. Selecting the specific user session and navigating to the user actions, the defined user action will be easy to identy goal by searching the [🏁] symbol.
![Conversion-Goal](./images/Conversion-Goal.png)

