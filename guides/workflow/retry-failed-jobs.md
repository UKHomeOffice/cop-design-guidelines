---
category: Workflow
expires: 2020-12-31
order: 3
---
# Retry Failed Jobs via REST API

## Introduction
There may be occasions when there is an error in the execution of a process instance. Camunda will create a job and an incident associated with the error, for example an external API was down, or there was a problem with the process variable which meant the process couldn't execute. In this situation you would normally go to Cockpit and look at the incident associated with that process instance. Cockpit will show you the underlying stack trace and visually show you where the failure has occurred. As a rule you would use Cockpit to retry the failed job, however, Cockpit and the actual runtime workflow are separate services. Therefore retrying in Cockpit may not work because of missing code or configuration. **Do not copy any code from the workflow engine into cockpit. We recommend you use Cockpit to update process variables or update task details.** When you need to retry the job, use the Workflow Engine APIs to do so.

Full details of Cockpit can be found [here](https://docs.camunda.org/manual/7.12/webapps/cockpit)

## An example of retrying a failed Job


Go to Cockpit, find the process definition, and associated running instances. You will then see failed process instances (Figure 1).

![Failed job]({{ '/images/failed-job-cockpit.jpeg' | relative_url }})

**Figure 1. Failed process instance**


Click on 'incidents' tab and it will show the underlying exception (Figure 2).

![Failed job details]({{ '/images/failed-job-details.jpeg' | relative_url }})

**Figure 2. Failed job**

For example 'emails' is missing. So you need to create a new process variable 'emails'. You can follow [this link](https://docs.camunda.org/manual/7.12/webapps/cockpit/bpmn/process-instance-view/#add-variables) to see how to add a process variable.

![Add process variable]({{ '/images/add-process-variable.jpeg' | relative_url }})

**Figure 3. Adding a process variable**

In the 'variable name' add 'emails' and then in the 'variable value' add a comma separated string of emails e.g. 'test@email.com, test2@email.com'. The value depends on your use case. Click 'add' and then use the REST API to retry the job.

You need to find the job details in order to retry the error. Use the following URL ```<<workflow-engine-url>>/rest/camunda/job?processInstanceId=aProcessInstanceId``` to get the job details.


This should give back something like this:
```json
[{
    "id": "aJobId",
    "jobDefinitionId": "aJobDefinitionId",
    "dueDate": "2018-07-17T17:05:00.000+0200",
    "processInstanceId": "aProcessInstanceId",
    "processDefinitionId": "aProcessDefinitionId",
    "processDefinitionKey": "aPDKey",
    "executionId": "anExecutionId",
    "retries": 0,
    "exceptionMessage": "Unknown property used in expression 'emails'",
    "failedActivityId": "anActivityId",
    "suspended": false,
    "priority": 10,
    "tenantId": null,
    "createTime": "2018-05-05T17:00:00+0200"
  }]
```

You need to copy the 'aJobId' and perform an HTTP PUT at the following address

```
<<workflow-engine-url>>/rest/camunda/job/aJobId/retries
```

The body of this request must be a valid JSON with the following data:
```json
{"retries": 1}
```
Once that has been submitted successfully, Camunda will retry the failed job, based on the number of counts you have added.
