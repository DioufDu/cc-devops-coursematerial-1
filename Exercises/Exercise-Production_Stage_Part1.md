#Exercise: Production Stage Part 1 - Download Artifact from Nexus

##Content
Now that we've uploaded our tested artifacts to Nexus, we can start downloading it from there. In this exercise you will download the war file from the nexus into the local jenkins workspace. This war file will be later used in the next exercise to deploy to the production space. 

##Step 1: Create Job `4-1-Production-Deploy`
- Create a freestyle job. 
- Add a String parameter called `ARTIFACTS_DIR`.
- In `Build` section click `Add build step` and select `Execute shell` from the list. 
- We will need the `version.properties`, `pom.xml` and `manifest.xml` from the archived artifacts. So let's first copy them from the `$ARTIFACTS_DIR`. Copy and paste the following shell scripts code. 
```SHELL
cp $ARTIFACTS_DIR/version.properties .
cp $ARTIFACTS_DIR/manifest.xml .
```
- However, this time, we will download the application's war file directly from nexus instead of the Jenkins workspace. Copy following shell scripts code and paste it directly behind the codes above. (change the `<place-holder>` in the codes according to your data.):
```SHELL
mkdir -p target

export MVN_ARTIFACT_VERSION=`cat version.properties | grep -oP '\d+\.\d+\.\d+-\d+-\d+_\w+'`
export MVN_GROUP_ID=com.sap.cc.<your-id>
export MVN_ARTIFACT_ID=bulletinboard-ads
export MVN_REPO_URL=http://nexus.wdf.sap.corp:8081/nexus/content/repositories/sap.milestones.manual-uploads.hosted/ 
#Downloads from Nexus to local maven repository
mvn -B org.apache.maven.plugins:maven-dependency-plugin:2.1:get -DrepoUrl=$MVN_REPO_URL -Dartifact=$MVN_GROUP_ID:$MVN_ARTIFACT_ID:$MVN_ARTIFACT_VERSION:war

#Copies from local maven repository to Jenkins workspace
cp ~/.m2/repository/com/sap/cc/<your-id>/bulletinboard-ads/$MVN_ARTIFACT_VERSION/bulletinboard-ads-$MVN_ARTIFACT_VERSION.war ./target/bulletinboard-ads.war
``` 
- Test the job by passing the parameter `$JENKINS_HOME/jobs/1-1-Commit/builds/lastSuccessfulBuild/archive`. You should be able to find the war file in your maven repository (`/home/vagrant/.m2/repository/com/sap/cc/<your-id>/bulletinboard-ads/`) as well as the jenkins workspace (`/home/vagrant/.jenkins/jobs/4-1-Production-Deploy/workspace/target`).
##Step 3: Deploy the Application in Production Space
- Now we have everything in the jenkins workspce, we can deploy the application to production. Adjust the following codes and paste them after the part of nexus downloading. 
```SHELL
cf login -u <CF_USERNAME> -p <CF_PASSWORD> -a https://api.cf.sap.hana.ondemand.com -o <your-org-name> -s <your-production-space>
cf create-service postgresql-9.4-lite free postgres-bulletinboard-ads
cf create-service rabbitmq-3.5.6-lite free bulletinboard-mq
cf push -n bulletinboard-ads-production-<your-id>
```
##Step 2: Trigger the job from Acceptance stage
- Configure the job `3-2-Acceptance-Release`
- Add a trigger as post build step, in which the `4-1-Production-Deploy` job is called
- Pass `ARTIFACT_DIR=$ARTIFACT_DIR` as predefined parameter of the `4-1-Production-Deploy` job.


