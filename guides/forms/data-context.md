---
category: Forms
expires: 2020-12-31
order: 4
---
# Data Context interpolation

## Introduction

When designing a form there are certain pieces of information you may want to be pre-populated in the form with dynamic data. These could be e.g. email details, phone numbers, or information from a business process.

Data context interpolation is the process whereby the expressions in a form are resolved to values. So, for example, when building a holiday request form, you want the form to automatically load 'requester's name', using the information of the person logged in and using the form.

In order to do that go to the form builder and select 'text field component'. Next give the 'Label' a name e.g. 'Requester name'.

![text field component]({{ '/images/text-field-component.jpeg' | relative_url }})

**Figure 1. Text field component**

Next select the 'Data' tab and click on 'Custom Default Value'.

![data context name]({{ '/images/data-context-name.jpeg' | relative_url }})

**Figure 2. Expression that pre-populates 'requester name'**

 In the JavaScript box, enter the following code:

```
value = data.keycloakContext.givenName + ' ' + data.keycloakContext.familyName
```
In Formio there is a root object called 'data'. Inside 'data' will be a set of additional data information, such as the person currently logged in, environment details, and process variable data i.e. data that is currently stored within a process instance.

So, in the example above, when a user logs in and is presented with the form, one of the data contexts available is their Keycloak credentials. This will include their first name, last name, email and other information.

In the above code 'keycloakContext.givenName' tells Formio to look inside the keycloak data for the 'given name' of the user and their 'family name'. These two values are joined by the two plus signs and the single quotation marks (+' '+). This ensures the given and family name are displayed with a space between them.

Next click on 'Save', to save the component, and select 'Update' to update the form.


![data context name resolved]({{ '/images/data-context-name-resolve.jpeg' | relative_url }})


**Figure 3. Requester's name dynamically resolved**

**You can use any of the components to display dynamic values, but don't use the 'Default Value' box in the component. This is because a default value is supposed to be static not dynamic i.e. the same value will always be loaded in that box and it appears before Formio does any processing.**

**The above process is the same whether in eForms or COP.**

## Start forms and user task forms

A 'start form' begins a process. A 'user task' is an activity within the process that has been launched by the 'start form'.

A 'start form' does not yet have a process context, so the only information that can be pre-populated is information from Keycloak (this is true for both eForms and COP). The start form at the beginning of the process cannot have information from the process itself because this has not begun yet. Other forms that are a part of the BPMN can have a process context because the process has begun.

The 'start form' is the first page to be displayed. Once this form has been filled in and submitted a process instance has been created and the process has begun.

A set of data contexts are available.

## Data contexts

### keycloakContext

Keycloak information can be used in a process start form or a user task form. Below are the expressions that can be used to generate a specific value e.g. a user's first name or email address. The first is for COP UI:
```
data.keycloakContext.accessToken
data.keycloakContext.sessionId
data.keycloakContext.email
data.keycloakContext.userName
data.keycloakContext.givenName
data.keycloakContext.familyName
data.keycloakContext.subject
data.keycloakContext.authServerUrl
data.keycloakContext.realm
```
All of the above can be used with eForms plus two additional expressions:
```
data.keycloakContext.roles
data.keycloakContext.groups
```

### environmentContext (only for use with COP UI)

The following values are usually used to produce dropdown lists by using a reference Url which then dynamically populates the list. This means the API Url does not have to be 'hard coded' as this may change from environment to environment.  

```
data.environmentContext.referenceDataUrl
data.environmentContext.workflowUrl
data.environmentContext.operationalDataUrl
data.environmentContext.privateUiUrl
data.environmentContext.attachmentServiceUrl
```

If you have a select box that needs to be populated from  external reference data then you can use the above in the 'Data Source URL'.

![data environment context]({{ '/images/data-environment-context.jpeg' | relative_url }})

**Figure 4. Data Source URl with data environment context expression**


For eForms if you need reference data enter:

```
/refdata/v2/entities/currency
```
The following diagram shows how you can use the attachmentServiceUrl within a file component.


![attachment url]({{ '/images/attachment-url.jpeg' | relative_url }})

**Figure 5. Attachment URL**

The file component allows users to attach files to the form. For more information on file component see [here]({{'/guides/forms/file-component/#file-component' | relative_url }})

### processContext

A form that is part of a user task is furnished with additional information about the process. This is process data that exists within the process i.e. information about the process and the specific instance of this process, and any variables associated with that instance.

This allows data that has been submitted by the start form to be available within the user task form.

#### eForms

In eForms there are the following standard context data:




```
data.processContext.instance.id
data.processContext.instance.definitionId
data.processContext.instance.businessKey
data.processContext.instance.caseInstanceId
data.processContext.instance.ended
data.processContext.instance.suspended
data.processContext.instance.tenantId
```
The keys after processContext.instance are described [here](https://docs.camunda.org/manual/latest/reference/rest/process-instance/get/)


```
data.processContext.definition.id
data.processContext.definition.key
data.processContext.definition.category
data.processContext.definition.description
data.processContext.definition.name
data.processContext.definition.version
data.processContext.definition.resource
data.processContext.definition.deploymentId
data.processContext.definition.diagram
data.processContext.definition.suspended
data.processContext.definition.tenantId
data.processContext.definition.versionTag
data.processContext.definition.historyTimeToLive
data.processContext.definition.startableInTasklist

```
The keys after processContext.definition are described [here](https://docs.camunda.org/manual/latest/reference/rest/process-definition/get/)

The data from the process instance can be accessed by using:

```
data.processContext.variables
```

followed by the name of the variable and the particular field of the variable e.g.

```
data.processContext.variables.holidayRequest.requesterName
```
This means there is a variable called 'holidayRequest' and inside that is a field called 'requesterName'.


### taskContext

This is the information about the user task i.e. the name of the task, the description, who it was assigned to, when it was created, and when it is due.

```
data.taskContext.id
data.taskContext.name
data.taskContext.assignee
data.taskContext.created
data.taskContext.due
data.taskContext.followUp
data.taskContext.description
data.taskContext.priority
data.taskContext.formKey
```
More information about the tasks can be found [here](https://docs.camunda.org/manual/latest/reference/rest/task/get/)
