---
category: Naming conventions
expires: 2020-12-31
---

# Naming conventions

## Introduction

There are different aspects of the platform: a form, BPMN, custom APIs. So it's important as you go through building forms and BPMNs that there's a consistent strategy that you apply. This will allow you to have meaningful data that is persisted in the data store.

### Forms
When you design a form there are two aspects to the forms

* how the form is presented to the user i.e. the labels and form components that a user has to fill in.

* the way in which the data payload is stored in the back end system.

Your user researcher should work with you to decide how the form is presented to the user.

 Your data architect/strategy team will provide the guidelines for the data structure.

#### General guidance

When deciding on naming conventions for the forms you will consult with your data architect/strategy team, but there are some general guidelines.

* API keys must be camelCase.

* API keys must have context - i.e. don't create general labels such as 'emailAddress'. Instead be precise e.g. 'lineManagerEmail'.

* Group API keys using 'form containers' i.e. the information should be grouped in a useful way e.g. all the 'officer' details such as 'email' and 'phone number' should be in a container 'officerDetails', all the 'line manager' details should be in another container 'lineManagerDetails', rather than all the email addresses being in one container, and all the phone numbers being in another.

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
For other details e.g. 'Line manager details' perform the same steps but this time create a 'Container component' with the API name e.g. 'lineManager'. You can add an email component and give it the same API key, 'email'. All the elements relating to the line manager's details will be in the 'Line manager details' container.

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
