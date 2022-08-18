## Preconfiguration Setup
We will be using dockers to stimulate and remediate the issues, so ansible-tower docker will communicate with the other dockers using docker-network.
In order it works as expected, start with checking if you already have a network present by issuing `docker network ls`
If not, create a network by issuing the command `docker network create mynetwork`

Now, run **ansible-tower docker** as`docker create -v /var/lib/postgresql/9.6/main --name tower-data nikhilgoenka/ansibletower /bin/true`

The above would create a postgres data volume which can be used to retain and retrieve the ansible-tower data. Once, the volume is created, run the ansible-tower docker as `
```bash
docker run -d --network mynetwork --name ansible-tower --volumes-from tower-data -p 8090:443 nikhilgoenka/ansibletower
```

From within your terminal, let us install **python-docker** on the hosts so that ansible-tower can perform operations on the host by running the following command `pip install docker`

We will use d1pacmworkshop user to access/change/modify docker, so give it required permissions on the docker. To do so, run `sudo usermod -aG docker d1pacmworkshop`
This would add d1pacmworkshop user to docker group and it will get the required permissions to access/change the docker.

Lastly, navigate to `/home/ubuntu/ACMD1Workshop/additional_resources/app_docker/scripts folder` and run
```bash
wget https://raw.githubusercontent.com/nikhilgoenkatech/AIOPSAnsibleBankPlaybooks/main/synthetic-monitor.sh
```
This will download the synthetic scripts that would be run in order to validate the auto-remediation.

Connect to "https:/images/Ansible-tower-browser-advanced-option.png)

Run the docker for **SampleBankApp** as below:
```bash
docker run --network mynetwork -d --name SampleBankApp -p 3000:3000 nikhilgoenka/sample-bank-app:1.0
```
Access the sample-bank app from your browser at http://<MACHINE-IP>:3000/login to populate the service "node-bank" under "Transactions & Services"

