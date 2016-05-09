# Exercise: ScmSyncPlugin
The ScmSyncConfiguration-Plugin for Jenkins allows to automatically store any change done to Jenkins configuration in a Git repository. This solution combines backup and versioning of Jenkins configuration. For ease of creating Jenkins servers, this exercise uses Docker and assumes, that the Docker/Jenkins (TODO: Name) exercise has already been completed. Note that this is the ScmSyncPlugin can be used with any kind of Jenkins server running on arbitrary infrastructure.

 

## Content

## Step 1: Setup ScmSync to archive your Jenkins server
 - Since you might be having multiple Jenkins server at this point, we want to backup the server running directly on your virtual machine. This is the server you did most of the exercises with and has configured access to your GitHub account.
 - Install the scm-sync-configuration plugin (Jenkins -> Manage Jenkins -> Manage Plugins)
 - Open GitHub in a browser and create a repository in your users organization (name 'jenkins-backup'). Copy the SSH-url.
 - Goto Jenkins -> Manage Jenkins -> Configure System and scroll to the `Scm sync configuration` section
   - Select Git and paste the Git repository URL into the corresponding `Repository URL` field.
   - Check `Never bother with commit message` - We will dive into this later
   - Check `Display SCM Sync status`
   - Save options
 - Now, there should be a bar on the bottom of your jenkins displaying a "sunny" status
 - Check your repository on GitHub
 
## Step 2: Explore configuration of ScmSync
 - Goto Jenkins -> Manage Jenkins -> Configure System and scroll to the `Scm sync configuration` section and uncheck `Never bother with commit messages`
 - Edit a job or create a new job and save it
 - A popup opens in which you're prompted to enter a commit message
 - After entering the message and committing, check your repository on GitHub
 
##Remark: Limitations of the SCM sync plugin
The SCM Sync Configuration Plugin has some Limitations you need to beware of:
 - Only a single Jenkins SCM Sync Configuration Plugin should write to this repository
 - No manual commits on this repository
 - No branching supported by SCM Sync
 - Or else SCM Sync will fail to push the changes and manual steps are required to restore SCM Sync Configuration  

## Step 3: Create a new Jenkins server and restore the old configuration
Due to those limitation, first, stop your currently running Jenkins server (with the SCM Sync Plugin). Then, we want to create a new Jenkins server via Docker and restore configuration of the current Jenkins. (It is also possible to just rename the $JENKINS_HOME folder on your vm and use the same Jenkins):
 - As a base, we use the Jenkins Dockerfile from Exercise `Docker` TODO: link correct exercise
 - From a terminal: Create a new folder `ScmSyncRestore` and `cd` into it
 - Create a new Dockerfile with `gedit Dockerfile`
 - Paste the following and save the file
 ```
 FROM `jenkins-training` TODO: Name of the image (in artifactory?)
 
 #Sets the user for the following scripts to jenkins
 USER jenkins
 
 COPY start_jenkins.sh /var/jenkins_tools/tools/start_jenkins.sh
 RUN chmod +x /var/jenkins_tools/tools/start_jenkins.sh
 
 #Replaces the startup script from original jenkins image with our own
 ENTRYPOINT ["/var/jenkins_tools/tools/start_jenkins.sh"]
 ```
 - In the same folder, create a file start_jenkins.sh with the following content:
```
 #!/bin/bash
if [ -f "$JENKINS_HOME/config.xml" ]
then
  echo "** Already found mounted Jenkins config, skipping initial pull"
else
  echo "** No jenkins home found, checking out from repository"
  pushd $JENKINS_HOME
  pwd
  git clone <Link to your jenkins backup repository>
  #Since JENKINS_HOME is not empty, we cannot checkout directly to JENKINS_HOME, so a subfolder with the name of the repository is created by git. We now need to move the contents back to $JENKINS_HOME  
  mv <Name of your jenkins backup repository>/* .
  popd 
fi

 #Start the original jenkins start scripts and pass arguments
exec /usr/local/bin/jenkins.sh $*
```
- Build the image
- Startup your new docker container

