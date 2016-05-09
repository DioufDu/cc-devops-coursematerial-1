---- D R A F T ----

<b>Option 1: Upload directly via Maven goal 'deploy'</b>
* <b>SNAPSHOT: works</b>

* Jenkins job: http://mo-f8268fdb9.mo.sap.corp:8080/jenkins/view/Cloud%20Curriculum%20M4/job/RSR_cc-bulletinboard-ads-spring-jersey_upload_nexus_by_maven/

* Nexus SNAPSHOT (build): http://nexus.wdf.sap.corp:8081/nexus/content/repositories/build.snapshots/com/sap/cc/bulletinboard-ads/0.0.1-master-SNAPSHOT/
* Further information:<br>
-- https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html<br>
-- https://wiki.wdf.sap.corp/wiki/display/LeanDI/Nexus+Landscape#NexusLandscape-DeploymentIntoNexus<br>
<br>
* <b>MILESTONE: tbd</b><br>

* Jenkins Job

<br>
* <b>RELEASE: tbd</b><br>
xxx

<b>Option 2: Upload indirectly via ci-connect and central x-make Jenkins</b>
* All jobs for agile-se on xmake Jenkins server: https://xmake-dev.mo.sap.corp:8443/view/agile-se/ 

* <b>SNAPSHOT: works</b>

* Jenkins Job: https://xmake-dev.mo.sap.corp:8443/view/agile-se/job/agile-se-cc-bulletinboard-ads-spring-jersey-master-CI-linuxx86_64/
* Nexus Snapshot: http://nexus.wdf.sap.corp:8081/nexus/content/repositories/deploy.snapshots/com/sap/cc/bulletinboard-ads/0.0.1-master-SNAPSHOT/
* Nexus SNAPSHOT (deploy): http://nexus.wdf.sap.corp:8081/nexus/content/repositories/deploy.snapshots/com/sap/cc/bulletinboard-ads/0.0.1-SNAPSHOT/
* Further Information:<br>
-- The „PI Central Build Service“ can build Maven projects out of the box, if there is a pom.xml available:
Goto https://ci-connect.mo.sap.corp/ --> select Repo --> activate „PI Central Build Service“ --> https://xmake-dev.mo.sap.corp:8443 (wait until GithubScanner and JobGenerator job are ready)<br>
-- Details on ci-connect see here: https://wiki.wdf.sap.corp/wiki/display/CIwithGithub/Home<br>
-- Build and Release Services: https://wiki.wdf.sap.corp/wiki/display/CIwithGithub/Build+and+Release+Service<br>

<br>
* <b>MILESTONE: '...-parent' works fine, but '...-ads' still has a problem</b><br>

* cc-bulletinboard-parent
  - GitHub Repo: https://github.wdf.sap.corp/agile-se/cc-bulletinboard-parent
  - Jenkins Job (...-parent): https://xmake-dev.mo.sap.corp:8443/view/agile-se/job/agile-se-cc-bulletinboard-parent-OD-linuxx86_64/
  - Nexus: http://nexus.wdf.sap.corp:8081/nexus/content/repositories/deploy.milestones.xmake/com/sap/cc/bulletinboard-parent/
 
Status: As long as a new version is created per job execution, Nexus Upload works fine. (e.g CommitId: 7bc5f4bf2b704d7d36be134219805aaae3e38a66, VersionExtension: CD003, CD004, CD00x)

* cc-bulletinboard-ads-spring-jersey
  - GitHib Repo: https://github.wdf.sap.corp/agile-se/cc-bulletinboard-ads-spring-jersey
  - Jenkins Job (...-ads): https://xmake-dev.mo.sap.corp:8443/view/agile-se/job/agile-se-cc-bulletinboard-ads-spring-jersey-OD-linuxx86_64/
  - Nexus: http://nexus.wdf.sap.corp:8081/nexus/content/repositories/deploy.milestones.xmake/com/sap/cc/bulletinboard-ads/

Status: Dependency to parent only via dedicate version (not SNAPSHOT). Upload Nexus works with this (CommitID used: 75986e61fd002814efc5be2927d3a6cdabbb31c2). 
  
<br>
* <b>RELEASE: tbd</b><br>
xxx

SAP Jira: https://sapjira.wdf.sap.corp/browse/CLOUDCURRICULUM-100
