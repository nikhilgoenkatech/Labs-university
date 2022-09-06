## Session Replay
In this exercise, we will configure Session replay and check the session replay which captured the user session.

### Enable session replay

1. To enable Session Replay navigate to the **Frontend** menu
1. Select the application which needs to be configured.
1. Select the **Browse [...]** button **> Edit**.
1. From the application settings menu, select **Session Replay and behaviour > Session Replay** and turn on **Enable Session Replay**.

![Session-Replay](./images/Replay.png)

### Masking Session Replay
Session Replay implements masking functionality that ensures that private user information is either not captured at the time of recording and/or masked at the time of session playback.

This option allows a more customized approach to configure masking. The changes in the configuration can be made to suite the session-recording requirements which does not require any changes in the application code. The functionality is used to hide interactions with specific elements that might inadvertently reveal confidential end-user information. By default, dynatrace provides some rules that would mask all user input and make session-replay tool available to study UI functionality.
![Session-Replay](./images/Mask-Replay.png)

Now, remove all the rules so as to allow session-replay functionality to record the end-users input details too.

### Sample Session Replay with default settings
By default, it masks all input hence we will not be able to see any inputs in the playback.
![SessionReplay-Example-default](./images/SessionReplay-default.mp4)

### Benefits of Session Replay
Whilst using the default settings would help to identify any issues with the UI. To help use the session-property feature entirely, it is recommended to mask the sensitive information.
Doing so would help you to identify/rectify any issues with your application that would have been triggered by certain user-actions as seen below.
![SessionReplay-Example-with-error](./images/SessionReplay-with-error.mp4)

**NOTE**: In the above example, we removed all the masking options and allowed dynatrace to record everything that is not deemed sensitive.

Now, let us add a rule to mask the telephone details that the customer is filling in the Address form. To do so, navigate to **Applications >ECommerce Application > [...]Browser > Capturing > Session replay**, click on *block list* under *Recording masking rules* and add the below:
1. Select *Element*.
1. Add "#id_phone" for *CSS selector to identify the content element*
![Session-Replay](./images/SessionReplay-blockrule.png)
**NOTE**: We will need to remove all other options manually which are under *Block list*.

### Session Replay with block config
Now, run some user-actions on the application and navigate back to the tenant to identify that Dynatrace masked the telephone details as configured in our rule earlier.
![SessionReplay-Example-with-block](./images/SessionReplay-with-block.mp4)


