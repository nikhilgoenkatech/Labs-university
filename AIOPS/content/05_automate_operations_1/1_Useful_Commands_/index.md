## Useful Commands ✅

Positive
: To start the docker with sample application:
`docker run -d --name SampleBankApp -p 4000:3000 nikhilgoenka/sample-bank-app`
* This would start the docker on port localhost:4000 with docker name as **SampleBankApp**

Positive
: To start the jenkins docker:
`docker run -d --network mynetwork --name Jenkins-Dynatrace -p 8020:8080  -v /var/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock nikhilgoenka/jenkins-dynatrace-workshop`
* -d runs the docker in daemon mode.
* -p 8020:8080 - By default, jenkins docker would be running on 8080. Specifying **-p 8020:8080** binds the 8080 in docker to localhost on 8020. So, you can forward/listen requests from docker using `localhost:8020`.
* -v Bind mounts a volume.
By default, jenkins docker is maintaining the pipeline/data information in /var/jenkins_home.
Specifying **-v /var/jenkins:/var/jenkins_home** would mount the localhost:/var/jenkins directory so that the pipeline data is not lost once pipeline is re-started.
Specifying **-v /var/run/docker.sock:/var/run/docker.sock** will allow the jekins docker to leverage the dockerd running on localhost. This would be required since we are starting the
sample-app dockers while running the pipeline.

Positive
: To run the ansible-tower docker:
`docker run -d --name ansible-tower -p 8090:443 ybalt/ansible-tower`
This would start the docker on port **localhost:8090** with docker name as **ansible-tower**

**Other useful commands:**
* To **view all docker containers**: `docker ps -a`
* To **view the downloaded images** on localhost: `docker images`
* To **remove a particular image**: `docker rmi <IMAGE-NAME>`
* To **stop a docker**: `docker stop <CONTAINER-ID>`
* To **remove a docker**: `docker rm <CONTAINER-ID>`
* To **run a docker in interactive bash**: `docker run -it <CONTAINER> /bin/bash`
* To **delete all the unused images**: `docker system prune -a -f`
* To **pull a particular image**: `docker pull <docker-image>`
* Jenkins pipeline:
Command: `https://github.com/nikhilgoenkatech/JenkinsBankApp`

<!-- ------------------------ -->

