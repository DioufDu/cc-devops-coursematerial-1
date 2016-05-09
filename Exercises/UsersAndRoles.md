#Users and Roles 

##Content:
Cloud Foundry uses role-based access control (RBAC), with each role granting permissions in either an org or a space.

| Organization Roles  | Rights           |
|:-------------------:|:----------------:| 
| Org Manager         |Organization administration rights (full access)  |
| Org Auditor         |Organization view only rights |
| Billing Manager     |Manage billing information  |



| Space Roles    | Rights           |
|:--------------:|:----------------:| 
|Space Manager   | Space administration rights (full access) |
|Space Developer | Manage applications and services |
|Space Auditor   | Space view only rights  |

##Example: Assigning a Space Role:
To assign the space developer to a colleague in some space within your org:

```
cf set-space-role <user> <org> <space> SpaceDeveloper
```
