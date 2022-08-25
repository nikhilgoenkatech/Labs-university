## Automate-Operations - II
In this step, we will start the ansible-tower docker and access it from Dynatrace tenant.

Start the ansible tower, using the command within EC2 instance by issuing
```bash
docker start ansible-tower
```

Once started, you will now be able to access ansible-tower from "https://<my-IP>:8090/".  Click on "Advanced" followed by Proceed to **IP:8090**
![Ansible-Docker](../../../assets/images/Ansible-tower-browser-advanced-option.png)

This would invoke the login page. Please login into the Ansible-tower instance using the below credentials:
**Username**: admin
**Password**: dynatrace

