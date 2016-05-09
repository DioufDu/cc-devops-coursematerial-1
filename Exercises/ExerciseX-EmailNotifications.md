#Exercise: Email Notifications
Whenever an error occurs during the CD pipeline, it is imperative that the original committer of the change is notified of the problem. In this exercise, you'll see how Jenkins handles such email notifications.

##Step 1: Add Email-Notification
- Open your Commit job.
- Add a post-build action `Editable Email Notification` 
- Click Advanced Settings
- In addition to the trigger `Failure - Any`, add triggers `Unstable (Test Failures)` and `Fixed`
- For all three triggers, add Developers and Culprits as recipients. Click the help button for Developers and Culprits and understand the difference. 
- Commit anything that leads to an error in this job and check if an email is sent.
- Do the same for a job further down the chain (e.g. system test) and see if it works the same way.
