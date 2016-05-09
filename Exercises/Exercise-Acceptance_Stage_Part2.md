#Exercise: Acceptance Stage Part 2 - Nexus Upload

##Content
In this exercise, we're using maven to do a manual upload to nexus in order to mark our now qualified build as release and archive its contents.

##Overview

<img src="images/ExerciseOverview_3-2_AcceptanceStage.png" width="800" />

##Step 0: Edit your maven settings.xml
- On your VM, open finder and go to your home directory
- Open the subdirectory `.m2`
- There, open settings.xml (by right-clicking and selecting open with gedit)
- At the end of the settings.xml, just before `</settings>`, insert the following
```XML
	<servers>
		<server>
			<id>nexus_sap.milestones.manual-uploads.hosted</id>
			<username>sap.milestones.manual-uploads</username>
			<password>NonLeanDiArt!</password>
        </server>
	</servers>
```
- This allows to upload files to the manual uploads of nexus.

##Step 1: Create Job
- Create a freestyle job and name it `3-2-Acceptance-Release`
- Check `This build is parameterized`, add a `String Parameter` named `ARTIFACTS_DIR`.
- In section `Source Code Management` select `Git` and insert the ssh link to your forked repository. As `Credentials` select `M4User`
- In section `Build`, add an `Execute shell` build step.
- Set Release-Tag
```SHELL
# tag with RELEASE
export ARTIFACT_VERSION=`sed 's/ARTIFACT_VERSION=\(.*\)/\1/' $ARTIFACTS_DIR/version.properties`
export BUILD_TAG=BUILD_$ARTIFACT_VERSION
export RELEASE_TAG=RELEASE_$ARTIFACT_VERSION
git tag "$RELEASE_TAG" `git rev-list -n 1 "$BUILD_TAG"`
git push origin "$RELEASE_TAG"

#checkout pom from correct tag
git checkout "$RELEASE_TAG"
```
- Copy the file `pom.xml` from `ARTIFACTS_DIR` to the job workspace
```SHELL
cp $ARTIFACTS_DIR/pom.xml .
```
- Thereafter, insert the following code which performs the nexus upload. Replace `<place-holder>` with your own information. 
```SHELL
export MVN_DEPLOY_PHASE="org.apache.maven.plugins:maven-deploy-plugin:2.8.2:deploy-file"
export MVN_DEPLOY_URL="http://nexus.wdf.sap.corp:8081/nexus/content/repositories/sap.milestones.manual-uploads.hosted/"
export MVN_DEPLOY_REPO_ID="nexus_sap.milestones.manual-uploads.hosted"
export MVN_DEPLOY_FILE="${ARTIFACTS_DIR}/target/bulletinboard-ads.war"
export MVN_GROUP_ID=com.sap.cc.<your-id>
export MVN_DEPLOY_POM=pom.xml
mvn ${MVN_DEPLOY_PHASE} -Durl=${MVN_DEPLOY_URL} -DrepositoryId=${MVN_DEPLOY_REPO_ID} -DpomFile=${MVN_DEPLOY_POM} -Dfile=${MVN_DEPLOY_FILE} -DgroupId=${MVN_GROUP_ID}
```
- Start the job manually and test it.



