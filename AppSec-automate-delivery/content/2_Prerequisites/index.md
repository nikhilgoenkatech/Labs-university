## Pre-requisites

### Run/Restart our SampleBank application

First, stop any samplebank application by running the following commands:
```
$ docker stop SampleBankApp

$ docker rm SampleBankApp

$ docker network create mynetwork
```

Now, start the application by running the command as follows:
```
$ docker run -d --name SampleBankApp  -e DT_CUSTOM_PROP='ENVIRONMENT=Test' -p 4000:3000 nikhilgoenka/sample-bank-app
```

This would start the docker on port localhost:4000 with docker name as **SampleBankApp**. Connect to your application once so that dynatrace picks up the service by accessing `http://AWS-IP:4000`

### Jenkins docker
Now with our application already running, let us now start the jenkins docker:
```
$ docker run -d --network mynetwork --name Jenkins-Dynatrace -p 8020:8080  -v /var/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock nikhilgoenka/jenkins-security-workshop
```

#### Understading the command:
* -d runs the docker in daemon mode.
* -p 8020:8080 - By default, jenkins docker would be running on 8080. Specifying **-p 8020:8080** binds the 8080 in d
ocker to localhost on 8020. So, you can forward/listen requests from docker using `localhost:8020`.
* -v Bind mounts a volume.
By default, jenkins docker is maintaining the pipeline/data information in /var/jenkins_home.
Specifying **-v /var/jenkins:/var/jenkins_home** would mount the localhost:/var/jenkins directory so that the pipeli
ne data is not lost once pipeline is re-started.
Specifying **-v /var/run/docker.sock:/var/run/docker.sock** will allow the jekins docker to leverage the dockerd run
ning on localhost. This would be required since we are starting the
sample-app dockers while running the pipeline.

