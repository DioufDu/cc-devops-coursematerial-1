#Exercise: Multijob Plugin to group stages

##Content
The multijob plugins allows to group and even parallelize jobs in Jenkins. Each multijob consists of multiple phases. The phases are executed in order, and, if a single phase fails, usually the multijob fails (although this can be configured). Each phase again consists of multiple Jenkins jobs. Those jobs inside the same phase are executed in parallel.     

We're going to use the multijob plugin to group our stages.

##Prerequisites
- Make sure the Multijob plugin is installed on Jenkins

##Step 1: Create Multijob for integration stage
Currently, the integration stage consists of two jobs. `Deploy` and `Systemtest`
- Create a new job - multijob, name it `IntegrationStage`.
- Add a String parameter `ARTIFACTS_DIR` like we did before.
- Add a build step `Multijob Phase` and name it `Deploy`
- In Phase Jobs of this phase, put the job name of the `Deploy` job.
- Click Advanced, Add Parameters, Predefined Parameters and insert `ARTIFACTS_DIR=$ARTIFACTS_DIR`, passing the artifacts dir from the current job to the called job.
- Add another Multijob Phase and do the same for `Systemtest` job
- Configure your `Deploy` job and remove the post-build trigger for `Systemtest` inside the `Deploy` job. The multijob plugin now triggers the `Systemtest` job
- Configure your `Commit` job and change the post-build trigger from Integration Stage `Deploy` to the new multijob `IntegrationStage`. The parameters is still required. 
- Save and test your multijob by committing a change to GitHub.
