---
category: Workflow
expires: 2020-12-31
order: 5
---

# Roles and Teams in relation to BPMN

## Introduction: The difference between 'roles' and 'teams'

### 'role'

A role is defined in 'Keycloak' SSO. In order to define a role in 'Keycloak' you must discuss the role with the 'Keycloak Administrator'.

A role is assigned to an individual user. Once a user has logged into COP, after starting a shift, on navigating to the 'Forms' section, they are presented with a list of forms that their role authorises them to fill in. This list is filtered based on the user's role.
The form is linked to the process definition in the BPMN. The workflow service presents all the process definitions (as forms) that match the user's role. In summary the role performs three functions:

* One is determining whether or not you are authorised to submit a specific form in COP.

* The second function is determining whether the user is authorised see a process instance (a submitted form) within a case. (In terms of COP, a case represents a collection of process instances all linked by a unique reference e.g. Border Force reference. A case never ends because it's a container that organises together all the process instances that are triggered by an individual or another event over time.)

* The third function is determining which actions a user is authorised to perform on a case.


### 'team'

When a user logs in on a shift, they select the team they have been assigned to work in for that shift. A user's ID is always the same but their team will change.

On the COP dashboard there is a panel labelled 'tasks assigned to your team'. This presents a list of active tasks allocated to that team. These will include assigned and unassigned tasks. This list of tasks is driven by 'team' not 'role'. However only an individual within a team can take on a task.


![team tasks panel ]({{ '/images/team-tasks-panel.jpeg' | relative_url }})

**Figure 1. Team tasks panel**


## Building a process definition in terms of 'roles' and 'teams'

This example process definition is for the submission and approval of a timesheet. The workflow for process definition 'complete timesheet' is created using BPMN in the usual way.

![teams roles bpmn ]({{ '/images/teams-roles-bpmn.jpeg' | relative_url }})
**Figure 2. A simple timesheet approval workflow**

On the 'Process properties' panel in the 'General' tab it is necessary to fill in the 'Candidate Starter Groups' box with the correct role/roles (it could be a comma separated string) e.g. 'copge'(COP general user). This role is configured in Keycloak SSO. Please consult your technical architect and/or your user researcher for the correct role definition for your particular use case.

![teams roles starter configuration]({{ '/images/teams-roles-candidate-starter-configuration.jpeg' | relative_url }})

**Figure 3. Candidate starter groups configuration box**

In the BPMN there are two user tasks: 'review time sheet submission', 'approve time sheet' (this is just an example, in reality the BPMN will be more complex). When either of the user tasks is initiated you want it to be allocated to a particular team. In order to do this, on the 'User task' 'General' tab, you will need to set the 'Candidate Groups' configuration. In the 'Candidate Groups' box fill in the team code that has been set up in the reference data service. If a team code does not exist please consult the reference data team.

![teams roles user task ]({{ '/images/teams-roles-user-task-team.jpeg' | relative_url }})

**Figure 4. Candidate groups for the 'approve timesheet' task**

By completing these configurations, you have defined who can access that process definition (as a form) in COP, and therefore start it, and who can access already existing instances of that process in a case.
