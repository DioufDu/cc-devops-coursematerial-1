# Build a Job through Jenkins Remote Access API
In this exercise, you will write an email which enables the receiver to trigger a job via clicking a hyperlink. On a successful start of the job, you will be redirected to the build page of the job you want to start. You will use the  [Jenkins Remote Access API](https://wiki.jenkins-ci.org/display/JENKINS/Remote+access+API)

To submit a job, an HTTP POST on `JENKINS_URL/job/JOBNAME/build` is to be performed. If the job is parameterized, then on `JENKINS_URL/job/JOBNAME/buildWithParameters?PARAMETER=Value`. 

For us, it goes a little bit further, as we want to be redirecetd to the job page after the request is successfully sent. We need a little script for this. Since the default content in the editable email notification will not support the script, we need to generate the HTML page during the build of the job, archive the HTML file, and send the link to the file to the receiver so they can start it from client side. 

-  Go to the configuration of `2-2-Integration-Systemtest`, add `Execute Shell` in `Add post-build step`. Copy and paste the following code to the part of 
```SHELL
rm -rf trigger.html
touch trigger.html
echo "<!DOCTYPE html>">>trigger.html
echo "<html>">>trigger.html
echo "<head>">>trigger.html
echo "    <script src='http://ajax.googleapis.com/ajax/libs/jquery/1.7/jquery.js'></script>">>trigger.html 
echo "<script type='text/javascript'>">>trigger.html
echo "    var url = '/job/3-1-Acceptance-Deploy/buildWithParameters'; ">>trigger.html
echo "    \$.ajax({">>trigger.html
echo "           type: 'POST',">>trigger.html
echo "           url: url,">>trigger.html
echo "           data: {">>trigger.html
echo "               'ARTIFACTS_DIR' : '${ARTIFACTS_DIR}'">>trigger.html
echo "           },">>trigger.html
echo "           success: function()">>trigger.html
echo "           {">>trigger.html
echo "               window.location.replace('/job/3-1-Acceptance-Deploy');">>trigger.html
echo "           }">>trigger.html
echo "    });">>trigger.html
echo "</script>">>trigger.html

echo "</head>">>trigger.html

echo "<body>">>trigger.html
echo "</body>">>trigger.html

echo "</html>">>trigger.html
```
- In section `Add post-build action `, add `Archive artifacts`
- Add `trigger.html` in the input field
- Change the default content in the `Editable Email Notification` into the following 
```HTML
If this artifact should be tested, please click 
<a href="${JENKINS_URL}job/2-2-Integration-Systemtest/${BUILD_NUMBER}/artifact/trigger.html" >here</a>
to start the deployment.
</br>
</br>

```
- Save the job. 

## Step 2: Start the job with a relaxed Jenkins Content Security Policy
Ever since Jenkins 1.641, a Content-Security-Policy header is introduced to static files served by Jenkins. This header is set to a very restrictive default set of permissions to protect Jenkins users from malicious HTML/JS files in workspaces, /userContent, or archived artifacts. ([Jenkins Content Security Policy](https://wiki.jenkins-ci.org/display/JENKINS/Configuring+Content+Security+Policy)).  

- To enable the script we need to relax the content security policy in Jenkins. We have already done this for you by creating an environment variable `JENKINS_JAVA_OPTIONS`, and set the security option as `-Dhudson.model.DirectoryBrowserSupport.CSP=script-src ajax.googleapis.com  'unsafe-inline'`. 
- Now stop the your jenkins server. Then restart your it by entering `java "$JENKINS_JAVA_OPTIONS" -jar jenkins.war`. 
- Build the job `2-2-Integration-Systemtest` and check out the email. Click on the link and see whether the `3-1-Acceptance-Deploy` is started correctly. 


## Extra information: check out the security setting from Jenkins management
You can also test the security settings from your Jenkins settings:
- Go to Manage Jenkins->Script Console.
- Enter the following configuration and click `Run`
  - To get the property:
  ```GROOVY
System.getProperty("hudson.model.DirectoryBrowserSupport.CSP")
```
  - To set the property
  ```GROOVY
System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "script-src ajax.googleapis.com  'unsafe-inline'")
```

**HOWEVER:** Setting the content security policy this way is only temerary. It will be disabled once you shut down the jenkins server.
