---
category: Naming conventions
expires: 2020-12-31
---

# Naming conventions

## Introduction

There are different elements of the platform: a form, BPMN, custom APIs. So it's important when building forms and BPMNs to use a consistent naming convention. This will allow you to have meaningful data that is persisted in the data store.

### Forms
When you design a form there are two aspects to the forms that must be considered:

* how the form is presented to the user i.e. the labels and form components that a user has to fill in.

* the way in which the data payload is stored in the back end system.

Your user researcher should work with you to decide how the form is presented to the user.

 Your data architect/strategy team will provide the guidelines for the data structure.

#### General guidance

When deciding on naming conventions for the forms you will consult with your data architect/strategy team, but there are some general guidelines.

* API keys must be camelCase.

* API keys must have context - i.e. don't create general labels such as 'emailAddress'. Instead be precise e.g. 'lineManagerEmail'.

* Group API keys using 'form containers' i.e. the information should be grouped in a useful way e.g. all the 'officer' details such as 'email' and 'phone number' should be in a container 'officerDetails', all the 'line manager' details should be in another container 'lineManagerDetails', rather than all the email addresses being in one container, and all the phone numbers being in another.

#### Creating a container

Open the 'form builder', click on the 'Data' button.

![data panel]({{ '/images/data-panel.jpeg' | relative_url }})

**Figure 1. Data panel**

From the drop-down options select 'Container'.

![data panel]({{ '/images/container-option.jpeg' | relative_url }})

**Figure 2. Container option**


Drag and drop it into the 'Form component' area. A 'modal dialogue box' will open. Click on the 'Display' tab and write the name of the container in the 'Label' box. By default this label will not be presented on the form. The label should have a meaningful name e.g. 'officer details'.

![officer container label]({{ '/images/officer-container-label.jpeg' | relative_url }})

**Figure 3. Officer container label**

Next click on the 'API' tab and in the 'Property Name' box fill in e.g. 'OfficerDetails' in camelCase.

![container api property name]({{ '/images/container-api-property-name.jpeg' | relative_url }})

**Figure 4. Container API 'Property Name'**


Remember to save it at this point. Then click on 'Advanced' on the 'Display' tab and drag and drop an 'Email' component.
In the label fill in 'Officer email'. In the API tab call it 'email' (you do not need to call it 'officerEmail' because the email address will be automatically organised to sit under 'officerDetails').
For other details e.g. 'Line manager details' perform the same steps but this time create a 'Container component' with the API name 'lineManager'. You can add an email component and give it the same API key, 'email'. All the elements relating to the line manager's details will be in the 'Line manager details' container.

The final form layout will look like this:

![form layout]({{ '/images/form-layout.jpeg' | relative_url }})

**Figure 5. Form layout**

When you submit the form the data that gets submitted to the back end system will look like this:

```json
{
  "data": {
    "officerDetails": {
      "email": "officer@example.com"
    },
    "lineManager": {
      "email": "linemanager@example.com"
    },
    "submit": true
  }
}
```

'email' appears under each container heading 'officerDetails' and 'lineManager', making it easy to see which email belongs to which.
This can be done for all personal details such as telephone number, date of birth etc.

### BPMNs

When building your BPMNs there is a set of guidelines and conventions you should follow to give your BPMN context and meaning. It is easy to name processes in a way which appears to be helpful to others and you at a later date, but is actually too unspecific and will ultimately be confusing e.g. naming a process 'investigate' seems specific enough but anybody else would not know the context or the particular meaning. A better name would be 'investigate customer claim'. The context of the process and its meaning is immediately apparent. This clear naming, taking account of context and meaning, will help other developers to quickly identify what the various processes of your BPMN do.

#### Activity names

A task or activity should be labeled with verb and noun. For example 'Investigate customer claim' or 'Approve holiday request'. The activity name should use a noun to describe what the task is doing.

Don't use general or unspecific verbs e.g. 'process', 'handle'. Make sure the verbs you use describe what is being done e.g. 'approve', 'submit'.  

#### Event names

An event is a BPMN notation that represents things that have happened or are supposed to happen. E.g. 'after fifteen minutes trigger alert', 'an email has arrived'. To find out more about events go [here](https://camunda.com/bpmn/reference/).

Always name an event by using a noun and a verb that describe a state e.g. 'email sent'. In addition try to describe the state of the noun when the process is about to leave the event e.g. 'holiday approved'.

![holiday request bpmn]({{ '/images/holiday-request-bpmn.jpeg' | relative_url }})

**Figure 6. Holiday request bpmn**

#### Gateway names

Alway name a decision gateway with a question. Each outgoing sequence should provide an answer to the gateway question. 

![naming gateways]({{ '/images/naming-gateways.jpeg' | relative_url }})

**Figure 7. Appropriately named gateway and outgoing sequences**

#### Process names

The process pool should be given the same name as the process the pool contains. So if the process in the pool is 'Approve holiday' then the pool would be called 'Holiday approval'. The name of the organisational role in charge of the process should also be included. In this case 'Manager'.

![pool name]({{ '/images/pool-name.jpeg' | relative_url }})

**Figure 7. Appropriately named pool**

Pools may have more than one lane, in which case the pool should have a name that reflects the whole process and each lane should be labelled with the role or technical system responsible for carrying out the activities in that lane. So if the pool is titled 'Holiday request and approval' then one lane would be labelled 'Employee' and the other 'Manager'.

![multilane pool]({{ '/images/multilane-pool.jpeg' | relative_url }})

**Figure 7. Appropriately named multi-lane pool**

#### General points

* Use sentences when naming BPMN symbols. So only capitalise the initial word (apart from proper nouns).E.g. 'Holiday request and approval'.

* Avoid using technical terms that would not be clear to all readers. So don't use a class name that appears in the code but instead every day language that any non-programmer would understand. 'Store data to S3' is not a good BPMN activity name. 'S3' is a technical construct and not all readers would understand what it is. Instead write 'Store to database'.

* Do not use abbreviations. It's easy to fall into using company or department specific abbreviations, but these should be avoided as they may be confusing to other readers.


### Environment properties

Your BPMN will require access to certain properties that are part of the runtime environment. This is because as the BPMN executes it accesses the context provided by the properties in the environment. These are defined in AWS Secrets Manager. The management and configuration of AWS Secrets Manager is owned by your DevOps engineer. If you require a new property in the Secrets Manager for your BPMN, e.g. an email address, then you should define the property and give it to your DevOps. Remember to specify which service requires the property.  

#### Creating/Requesting new properties

* Check there is not already a property in Secrets Manager that you can use. You may need to consult your Lead Developer.

* Once you are certain you need a new property, make sure you use the correct naming convention.  

#### Naming convention

The first part of the name is the technical service name e.g gov.notify.template

The second part of the name will be the product name e.g. buildingPass.

The final part of the name is the activity of your BPMN e.g. submitApplication. This is specific to your BPMN but you must make sure you use the same name in your BPMN as your DevOp set up in the Secrets Manager.

To ensure that the BPMN fails if the property isn't present in Secrets Manager use the following expression:

```
${environment.getRequiredProperty('gov.notify.template.buildingPass.submitApplication')}

```

This will make sure your BPMN fails immediately, so you know either you called the wrong property, or the property you require is not in the Secrets Manager.
