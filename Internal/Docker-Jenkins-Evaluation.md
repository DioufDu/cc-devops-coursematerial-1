# Evaluate running Jenkins in Docker


## Install Docker and set proxies

**Links**: 

[Official Instruction](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

[Tutorial Instrtuction](https://docs.docker.com/linux/step_one/)

- Install docker `curl -fsSL https://get.docker.com/ | sh`
- Add docker user group to avoid using `sudo` everytime
```
sudo groupadd docker
sudo gpasswd -a ${USER} docker
```
- Restart VM
- Add proxy settings. Run `sudo vi /etc/default/docker`. 
- Click `i` to start editing. Copy paste the following to the file. click `esc` and type `ZZ` to save the file and exit the editor. 
```
export https_proxy="http://proxy.wdf.sap.corp:8080"
export http_proxy="http://proxy.wdf.sap.corp:8080"
export no_proxy=".local,169.254/16,*.sap.corp,*.corp.sap,.sap.corp,.corp.sap,localhost,nexus"
```




## Option 1: Use official Jenkins docker image
- [Docker image on Dockerhub](https://hub.docker.com/_/jenkins/)
- [Dockerfile of the image](https://github.com/jenkinsci/docker/blob/e7d56fa71d44ace1f1d8258ef216bba64f266cca/Dockerfile)

### Use Case Description:
- Find out how to build a docker image, create and start a container
- Find out how to utilize Dockerfile to add additional functions in the Jenkins basis image

### Download Jenkins Official image and run it
- Download an official jenkins docker image
  `docker run jenkins`
  - `docker run <image-name>` will first look whether you have the image locally. If not, it will be downloaded from Dockerhub.
  
- Create a docker container and start it
  - `docker run` will create a new container and start it. You can define many parameters in after the `run`. learn more [here](https://docs.docker.com/engine/reference/commandline/run/)
 
    - E.g. `docker run --name <container-name> -p <host-port>:<container-port> -v <host-dir>:<container-volume> Jenkins`
    - This creates and starts a container from image `Jenkins`, forward the port to the host and make that a persistent volume in your host machine. 
  - `docker create` and `docker start`
    - `docker create`generates a new container. You can also define port forwarding, volume...here, so when you start the container, the parameters will be taken accordingly. See more [here](https://docs.docker.com/engine/reference/commandline/create/)
    - `docker start` starts a given container. You can NOT define the port, volume...here. See more [here](https://docs.docker.com/engine/reference/commandline/start/)
    - E.g. 
    
      ```
      Docker create -p 8080:8080 -v <host-dir>:<container-volume> --name <container-name> Jenkins
      Docker start <container-name>
     ``` 
     These two lines have the same effect like the one above. 
     
### Write a Dockerfile to customize the Base Jenkins Image: install maven and a list of jenkins plugins
- Using the base image : `FROM jenkins:<version-number>`
- Setting the proxy: 

```
ENV http_proxy http://proxy.wdf.sap.corp:8080
ENV https_proxy http://proxy.wdf.sap.corp:8080
```

- Change user right: `USER root`. By default the user doens't have root access. If one want's to make folder in `/var`, it is not allowed. 
- Run the shell script : `RUN <shelll command>`. The following codes install maven: 

```
RUN mkdir -p /var/jenkins_tools/tools/maven
WORKDIR /var/jenkins_tools/tools/maven
RUN wget http://ftp.halifax.rwth-aachen.de/apache/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz -O maven.tar.gz
RUN tar xzvf maven.tar.gz --strip-components=1
RUN rm -rf maven.tar.gz
ENV M2_HOME=/var/jenkins_tools/tools/maven
ENV PATH="$M2_HOME/bin:$PATH"
```

- Copy a file from host to image: `COPY`. The file in host system has to be in the same or subordinate folder of the dockerfile. e.g. The following codes copy a shell script which downloads the jenkins plugins and dependencies. A list of to be installed plugins are saved in a txt file. 

```
RUN mkdir -p  $temp_folder/plugins/
COPY download_plugins.sh /var/jenkins_tools/tools/download_plugins.sh
COPY get_plugin.sh /var/jenkins_tools/tools/get_plugin.sh
COPY plugins.txt /var/jenkins_tools/tools/plugins.txt
RUN /var/jenkins_tools/tools/download_plugins.sh /var/jenkins_tools/tools/plugins.txt

```
- Define`ENTRYPOINT` : `ENTRYPOINT ["/bin/bash"]`. The entrypoint defines how the container will be started. e.g. the base jenkins image has defined `ENTRYPOINT ["/bin/tini", "--", "/usr/local/bin/jenkins.sh"]`, means it uses a shell script which starts jenkins. With `ENTRYPOINT ["/bin/bash"]`, one accesses the container without starting any programm. It is the same as `docker run -i -t <image-name> /bin/bash`.



###### extra

- List and remove images
  - List intermediate images: `docker images`
  - List all images: `docker images -a`
  - Remove image : `docker rmi <image-name>/<image-id>`
  - Remove all intermediate images: `docker rmi $(docker images)`

**NOTE: ** The image can only be removed when all the dependent containers are removed.  
- List and remove containers
  - List running containers: `docker ps`
  - List all containers:`docker ps -a`
  - Remove container: `docker rm <container-name>`
  - Remove all containers: `docker rm $(docker ps -a -q)`

- inspect the folder parallel with jenkins running
```
docker exec -i -t jenkins_main /bin/bash 
```
