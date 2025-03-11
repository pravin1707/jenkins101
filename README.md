# Installation of Jenkins on Docker

**Installation of Jenkins on Docker to build Automation Server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.**

Jenkins can be installed through native system packages, such as Docker, or even run standalone by any machine with a Java Runtime Environment (JRE) installed.

The Jenkins project provides a Linux container image, not a Windows container image. Be sure that your Docker for Windows installation is configured to run Linux Containers rather than Windows Containers. 
Refer to the Docker documentation for instructions to [switch to Linux containers](https://docs.docker.com/desktop/setup/sign-in/#switch-between-windows-and-linux-containers). Once configured to run Linux Containers, the steps are:

The setup instructions are similar for both [windows](https://www.jenkins.io/doc/book/installing/docker/#on-windows) and [macOS and Linux](https://www.jenkins.io/doc/book/installing/docker/#on-macos-and-linux)
despite minor differences.

Below is the installation guide for windows: 

1. Open up a command prompt on your window and follow the instructions listed below:

2. Create a bridge network in Docker
```
docker network create jenkins
```

3. Run a docker:dind Docker image
```
docker run --name jenkins-docker --rm --detach `
  --privileged --network jenkins --network-alias docker `
  --env DOCKER_TLS_CERTDIR=/certs `
  --volume jenkins-docker-certs:/certs/client `
  --volume jenkins-data:/var/jenkins_home `
  --publish 2376:2376 `
  docker:dind
```

4. Customize the official Jenkins Docker image, by executing the following two steps:

a) Create a Dockerfile with the following content:
```
FROM jenkins/jenkins:2.492.2-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
https://download.docker.com/linux/debian \
$(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"
```

b) Build a new docker image from this Dockerfile and assign the image a meaningful name, e.g. "myjenkins-blueocean:2.492.2-1":
```
docker build -t myjenkins-blueocean:2.492.2-1 .
```

5. Run your own myjenkins-blueocean:2.492.2-1 image as a container in Docker using the following docker run command:
```
docker run --name jenkins-blueocean --restart=on-failure --detach ^
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 ^
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 ^
  --volume jenkins-data:/var/jenkins_home ^
  --volume jenkins-docker-certs:/certs/client:ro ^
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.492.2-1
```

6. Proceed to the [Setup wizard](https://www.jenkins.io/doc/book/installing/docker/#setup-wizard).

**Sources**

[Setup Guide](https://www.jenkins.io/doc/book/installing/docker/) - Detailed setup guide link (Jenkins official website)
