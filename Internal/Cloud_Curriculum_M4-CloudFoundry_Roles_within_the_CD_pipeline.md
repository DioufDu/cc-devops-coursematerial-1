# Cloud Foundry Role descriptions

## Organization Roles
* OrgManager - Invite and manage users, select and change plans, and set spending limits
* BillingManager - Create and manage the billing account and payment info
* OrgAuditor - Read-only access to org info and reports

## Space Roles
* SpaceManager - Invite and manage users, and enable features for a given space
* SpaceDeveloper - Create and manage apps and services, and see logs and reports
* SpaceAuditor - View logs, reports, and settings on this space

# Recommended Role Setup within the Spaces of the CD Pipeline

As we'd like to establish our deployments through the deployment pipeline we want to ensure that our application instances
within the Cloud Foundry system's spaces are roled out automatically and can not be influenced by accidental developer configuration changes. It is recommended that developers only have read access rights in all Spaces of the according CD pipeline.
In addition it is recommended to provide an experimentation place (e.g. test Space) for our developers to enable them to deploy test versions to Cloud Foundry manually.

This would be a recommended setup:

* PUser - OrgManager, SpaceDeveloper
* Developers - OrgAuditor
  * test Space: SpaceManager, SpaceDeveloper
  * integration, acceptance, production Spaces: SpaceAuditor
* Project Managers / PO - Organization Billing Manager, SpaceAuditor

# Current Limitations

Because of the bug described [here](https://github.com/cloudfoundry/cf-release/issues/693) ([NGPJiraTicket](https://jtrack.wdf.sap.corp/browse/NGPBUG-20213)) it
is not possible to assign OrgManager or SpaceManager roles to users except for Cloud Foundry Admin users.
This also prevents OrgManagers from managing spaces by themselves.

# Workaround Workflow

1. Create User at [Hana Platform](https://account.hana.ondemand.com/)
2. [Self-service Cloud Foundry registration](http://onboard.cfapps.neo.ondemand.com/)
3. Open Ticket to create/delete a space or assign specific roles to user within the [support ticket system](https://jtrack.wdf.sap.corp/secure/CreateIssue!default.jspa)

# Links

* Cloud Foundry Documentation - https://docs.cloudfoundry.org/concepts/roles.html
