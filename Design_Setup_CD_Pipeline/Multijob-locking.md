#Locking multiple Jenkins jobs with Multijob-Plugin
##Problem
When sharing test spaces across apps, it is necessary to prevent multiple deployments and/or tests from running at the same time. 
Even more so, the following race condition might influence tests: If app A and B almost simultaneously queue their jobs for deployment, the following order of execution is possible: A-Deploy, B-Deploy, A-Test, B-Test. Using locks inside any or all of those job does not prevent this race condition.

##Solution
To prevent such a mixup, it is necessary to take the lock before A-Deploy and keep it until A-Test is completed. We can create a multijob job (e.g. A-Integration) which runs A-Deploy and A-Test. This job can be set to lock a space specific lock (e.g. "Integration Lock"). If all jobs are setup in the same way, the integration stage is sequentialized.
