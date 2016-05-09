#Exercise: Production Stage Part 2 - Blue/Green Deployment
 
##Content
Blue/Green-Deployment defines a way to do zero-downtime deployment. In the following exercise, we first manually do a blue-green-deployment. Then, an unfinished ruby script is to be completed. Afterwards, this ruby scripts is called via Jenkins job in order to automate it.

##Step 0: Prepare a directory
 - The first part of this exercise is best done from a console inside the provided vm.
 - Open a new console
 - Create a new directory bluegreen `mkdir bluegreen`
 - Copy the artifacts from the last Commit-Job `cp -r .jenkins/jobs/1-1-Commit/builds/lastSuccessfulBuild/archive/* ~/bluegreen/`

##Step 1: Do manual blue-green-deployment
This part of the exercise is best done from a console inside the provided virtual machine, so you can see the immediate outcome of each command. In the following parts, adjust the `<place-holders>` accordingly.
 - Goto your bluegreen directory `cd bluegreen`
 - login to your org/production space `cf login -u <CF_USERNAME> -p <CF_PASSWORD> -a https://api.cf.sap.hana.ondemand.com -o <your-org-name> -s <your-production-space-name>`
 - Run `cf apps` and ensure that bulletinboard-ads is running. 
 - If the app is not pushed yet, `cf push bulletinboard-ads -n bulletinboard-ads-production-<your-id>`
 - Look at `cf apps` and notice the url of your app
 - Open a browser to `https://<url_to_your_app>.<domain>` and see if it runs.
 - Now, push `bulletinboard-ads-blue` and see if it runs: `cf push bulletinboard-ads-blue -n bulletinboard-ads-production-<your-id>-blue`
 - Map the main-route pointing to your current app `bulletinboard-ads` to `bulletinboard-ads-blue`: `cf map-route bulletinboard-ads-blue cfapps.sap.hana.ondemand.com -n bulletinboard-ads-production-<your-id>`
 - `cf apps`, see that there are two urls for `bulletinboard-ads-blue`. Notice the same route being used for `bulletinboard-ads` and `bulletinboard-ads-blue`. 
 - Unmap the route to bulletinboard-ads: `cf unmap-route bulletinboard-ads cfapps.sap.hana.ondemand.com -n bulletinboard-ads-production-<your-id>`
 - `cf stop bulletinboard-ads`
 - Congratulations, you did your first zero-downtime deployment. From now on, we use the route `bulletinboard-ads-production-<your-id>` as main-route and switch between apps `bulletinboard-ads-blue` and `bulletinboard-ads-green`

##Step 2: Automate everything
 - Run `ruby scripts/simple_blue_green.rb` and see that it fails on push
 - Open `<Forked Repository>/scripts/simple_blue_green.rb` in github, analyze the ruby-script and fill in the missing CF-commands (analogous to step 1) marked with  `#TODO`
 - Hint: You need to only fill in the Cloud Foundry commands into the positions marked with `#TODO`. Look at the parameters of the functions where you insert the Cloud Foundry commands. All of the parameter should be inserted into the CF command like `#{parameter_name}`. See function `login` for an example.
 

##Step 3: Run the script on Jenkins in Production-Stage
 - At the beginning of the shell script inside your Production-Deploy-job, copy those scripts from the archive directory to the workspace
```SHELL
rm -r -f scripts
mkdir -p scripts
cp $ARTIFACTS_DIR/scripts/* ./scripts 
```
 - Replace the deployment part of your current shell-script with `ruby scripts/simple_blue_green.rb <your_org_name> <your_production_space_name> <CF_USERNAME> <CF_PASSWORD>`.
 - Run your production-stage
