## Prerequisites

In this exercise , we will deploy the sample web application.

Negative
: Use PuTTy (Windows), PowerShell (Windows) or Terminal (Mac), ssh into the instance (IP address using the your PEM Key)

Connect to the EC2 instance using the following credentials:
**Username**: d1prumworkshop
**Password**: dynatrace

Now, gain root access by performing `sudo su`
*Hint: Password for root is also dynatrace*

### Deploy the Application on your AWS Instance

* Navigate to the `/home/ubuntu/e-commerce/src` folder and run command
```
$ cd /home/ubuntu/e-commerce/src
$ source myenv
```
![step-1](./images/pre-step-1.png)

* Run the following commands to deploy your application:
```
$ python3.6 /usr/local/bin/gunicorn --bind 0.0.0.0:3005 ecommerce.wsgi:application &

$ service nginx start
```
![step-2](./images/pre-step-2.png)
![step-3](./images/pre-step-3.png)


### Accessing the Application UI
Within your **web browser**, access the sample application with `AWS IP ADDRESS`
![Application URL](./images/application-access.png)

<!-- ------------------------ -->
