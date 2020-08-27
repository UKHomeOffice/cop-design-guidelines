---
category: Forms
expires: 2020-12-31
order: 4
---
# Data Context interpolation

## Introduction

When designing a form there are certain pieces of information you may want to be pre-populated in the form with dynamic data. These could be e.g. email details, phone numbers, or information from a business process.

Data context interpolation is the process whereby the expressions in a form are resolved to values. So, for example,
when building a holiday request form, you want the form to automatically load 'requesters name', using the information of the person logged in and using the form.

![data context name]({{ '/images/data-context-name.jpeg' | relative_url }})

**Figure 1. Expression that pre-populates 'requester name'**

In order to do that go to the form builder and select 'text field component'. Next give the 'Label' a name e.g. 'Requester name'. Then select 'Custom Value' in the JavaScript box. Enter the following code:

```
value = data.keycloakContext.givenName + ' ' + data.keycloakContext.familyName
```
In Formio there is a root object called 'data'. In side 'data' will be a set of additional data information. Such as the person currently logged in, environment details, and potentially process variable data i.e. data that is currently stored within a process instance.

So in the example above when a user logs in and is presented with the form. One of the data contexts available is their Keycloak credentials, this will include their first name, last name, email and other information.

In the above code 'keycloakContext.givenName' means tells Formio to look inside the keycloak data for the 'given name' of the user and their 'family name'. These two values are concatenated by the two '+' the single quote marks ensure the given and family name are displayed with a space between them.

Next click on 'Save'. This saves the component. Select 'Update' to update the form


![data context name resolved]({{ '/images/data-context-name-resolve.jpeg' | relative_url }})


**Figure 2. Requester's name dynamically resolved**

**You can use any of the components to display dynamic values, but don't use the 'Default Value' box in the component. This is because a default value is supposed to be static not dynamic i.e. the same value will always be loaded in that box and it appears before Formio does any processing.**

The interpolation process is the same whether in eForms or COP.

A 'start form' begins a process. A 'user task' is an activity within the process that was begun by the 'start form'. A 'start form' does not yet have a process context so the only information that can be prepopulated is information from Keycloak (this is true for both eForms and COP). When you have a form that starts a process, you don't have a process context. A form associated with a user task has a process context. 

When selecting a form/process the first page to be displayed is a start form. At this point the process has not started so it is not possible to ask for a process context. Once this form has been filled in and submitted there is now a process instance and a process context.

At this stage if there is a 'user task' with a form associated with it

For example for the form 'holiday request'

A set of data contexts are available.
