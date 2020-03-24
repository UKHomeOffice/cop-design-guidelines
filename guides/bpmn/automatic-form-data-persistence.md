---
category: BPMN
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
