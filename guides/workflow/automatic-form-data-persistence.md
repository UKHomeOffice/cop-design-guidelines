---
category: Workflow
expires: 2020-12-31
order: 1
---
# Automatic Form Data Persistence to S3

## Introduction

The new release of the COP Workflow Engine enables automatic **form data** persistence to S3.
Previously in order to save data it would have been necessary to add a 'service task' to your BPMN at various points in the workflow or at the end.

This bloats your BPMN, is error prone, makes debugging difficult, and clutters the BPMN with technical constructs, which make it difficult for a non-expert user to implement.

Now all that is necessary is to create a user task and associate a form you've created with that user task. No further service tasks or code to persist that form data is required. The engine will implicitly handle this for you.

## Changing S3 bucket name

By default the workflow engine will persist the form data in an S3 'bucket' ```<ENVRIONMENT>-cop-case```. You can change this by going to the BPMN modeller and clicking on the 'Extensions' tab in the 'Process' section. Click on 'Add Property' in the 'Name' input box and type in ```product```. In the 'Value' box write the product name.

![Product name]({{ '/images/productName.png' | relative_url }})

**Figure 1. Changing S3 bucket name**

## Persistence failure

In the event of a failure to persist data to S3 an incident will be created in Cockpit for you to see. The incident will include a brief message and the underlying stack trace so that you can track down the issue. It is worth noting that in the event of a failure, form data is not lost, it is a process variable that is associated with a process instance.

## Disabling data persistence

If the default persistence behaviour does not work in your use case **(please consult with the Platforms Team)** you may disable data persistence. This should only be done as a last resort and in consultation with your Technical Architect/Lead Developer, as the default should be sufficient.

Once you have ascertained that you need to disable data persistence do the following: Go into the BPMN modeller click on the 'Extensions' tab in the 'Process' section. Click on 'Add Property' in the 'Name' input box and add the following ```disableExplicitFormDataS3Save``` and in the 'Value' box write 'true'.

![Disable automatic persistence]({{ '/images/disableAutomaticPersistence.png' | relative_url }})

**Figure 2. Disable automatic form data persistence S3**

## Storing data from an external source

In some instances it may be necessary to store data that has been collected from another form, that has not been created through a user task, or may even be data originating from an external system other than COP.

In this case you need to create a variable within the process instance that conforms to a particular convention. Once you create a variable using the convention, the workflow engine will automatically persist your data in S3 in the same manner as data associated with a user task/form.

In order to do this first create a variable, either by sending a message to a running process instance, or using the Camunda REST API. Please see [here](https://docs.camunda.org/manual/latest/reference/rest/) for more detailed API guidance. For the legacy COP engine you need the following JSON fragments somewhere within your existing JSON payload. Ordering of these fragments is not relevant as long as these are present in your JSON:

```json
{
  "customData": {

  },
  "form": {
    "formId": "",
    "formVersionId": "",
    "name": "myForm",
    "submissionDate" : ""
  },
  "process" : {
    "definitionId" : ""
  },
  "shiftDetailsContext": {
    "email": ""
  }
}

```
The essential elements are the 'form' section and the 'shiftDetailsContext'. This data will be stored in S3 but it can also be displayed in a form in COP or in 'case view'. In order to present it in 'case view' you will need a form. Therefore the 'formId', 'formVersionId', and 'name' are all required fields. At this point you may not know the 'formId', or the 'formVersionId'. As long as you know the 'name' you can make a call to the form API server to populate the 'formId' and 'versionId'.  

In the case of legacy COP, where 'shiftDetailsContext' is required the email can be a user email address, or a system account email address. These email addresses must exist in Keycloak SSO.

In the new COP engine (eForms) use the following:
```json
{
  "customData": {

  },
  "form": {
    "formId": "",
    "formVersionId": "",
    "name": "myForm",
    "submittedBy": "",
    "submissionDate": ""
  },
  "process" : {
    "definitionId" : ""
  },
}
```
For new COP 'submittedBy' should be a user email address or a system account.

 Find the Open API documentation required to get the 'formId' and 'formVersionId' [here](https://form-api-server.dev.cop.homeoffice.gov.uk/api-docs/swagger/#/Forms/get)

The actual API call you would need to make is:

```
http://form-api-server.dev.cop.homeoffice.gov.uk/form?limit=1&offset=0&name=myForm
```

This will give you a JSON collection with a single entry. From this you can get the 'formId' and 'formVersionId'.

In order to get the 'process definitionId' you will need to make a Camunda API call to get the 'process instance'. If you have the 'process instance ID' then you can make the following API call.

```
<WORKFLOW_URL>/process-instance/<PROCESS_INSTANCE_ID>
```

If you don't have the 'process instance ID' then you will need to use the business key to get the correct process instance. This can be done using the following:

```
<WORKFLOW_URL>/process-instance?businessKey=<BUSINESS_KEY>&maxResults=1&firstResult=0
```

This will give you a single item in a collection. You can then extract 'process definitionId' from the data.

Once this is configured, you will see your data in the 'case view' as a form, which can then be printed for offline use.
