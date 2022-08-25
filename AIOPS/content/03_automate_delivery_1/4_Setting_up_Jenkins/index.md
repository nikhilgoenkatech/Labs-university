## Setting up Jenkins
### Insalling Docker Pipeline Plugin
Once you login, you will see the screen as below. Click on "Install suggested plugins" as below:
![setup-Jenkins-1](../../../assets/images/jenkins-1.png)
Further, add an admin user with username "admin" and password "dynatrace"

![setup-Jenkins-2](../../../assets/images/jenkins-2.png)
* Click on **Manage Jenkins** on the left menu

![setup-Jenkins-3](../../../assets/images/jenkins-3.png)
* Click on **Manage Plugins** as highlighted below:

![setup-Jenkins-4](../../../assets/images/jenkins-4.png)

Now, click on **Available plugins** and input docker in the **search bar**
1. Select **Docker Pipeline**
2. Click on **Install without restart**

![setup-Jenkins-5](../../../assets/images/jenkins-5.png)

### Configure Environment Variables
Within Jenkins, click on **Manage Jenkins** > **Configure System**
![setup-Jenkins-6](../../../assets/images/jenkins-6.png)

* Look for **environment variables** as below:
![setup-Jenkins-7](../../../assets/images/jenkins-7.png)
![setup-Jenkins-8](../../../assets/images/jenkins-8.png)

* Add the following environment variables:
1. **DT_URL** with value *https://mou612.managed-sprint.dynalabs.io/e/{environmentid}*
2. **DT_TOKEN**
3. **PUBLIC_IP**

To get your ***DT_TENANT***, go to the Web Browser and extract the URL path as per below.
![setup-Jenkins-9](../../../assets/images/jenkins-9.png)

To generate your DT_TOKEN, go to Settings > Access Tokens and follow the below:
1. Create a token with **LoadTest**
2. Toggle **Data ingest**, eg: **metrics and events**
3. Toggle **Create and read synthetic monitors**
4. Also, toggle **read slo, write slo, problem feed**
5. Click on **Generate**
6. Clck on **Copy**
![setup-Jenkins-10](../../../assets/images/DT_TOKEN_Screenshot.png)

### Configure Jenkins Pipeline
* Click on "New Item" on the left side:

![setup-Jenkins-11](../../../assets/images/jenkins-11.png)
* Add a pipeline as per below:
* Item name - ***My Pipeline***
* Choose **Pipeline**
* Click on **OK**


![setup-Jenkins-12](../../../assets/images/jenkins-12.png)

* Use the pipeline definitions as per below:
* Definition - Dropdown **Pipeline script from SCM**
* SCM - Dropdown **Git**
* Repository URL - ***https://github.com/nikhilgoenkatech/JenkinsBankApp/***
* Click on **OK**

![setup-Jenkins-13](../../../assets/images/jenkins-13.png)

<!-- ------------------------ -->
