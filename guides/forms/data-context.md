---
category: Forms
expires: 2020-12-31
order: 4
---
# Data Context interpolation

## Introduction

When designing a form there are certain pieces of information you may want to be pre-populated into the form or dynamically loaded. These could be e.g. email details or phone numbers.

Data context interpolation is the process whereby the expressions in a form are resolved to values. So, for example,
when building a holiday request form, you want the form to automatically load 'requesters name', using the information of the person logged in and using the form.

![data context name]({{ '/images/data-context-name.jpeg' | relative_url }})

**Figure 1. Expression that pre-populates 'requester name'**

In order to do that go to the form builder and select 'text field component'. Next give the 'Label' a name e.g. 'Requester name'. Next select 'Custom Value' in the JavaScript box enter the following code:

```
value = data.keycloakContext.givenName + ' ' + data.keycloakContext.familyName
```
Next click on 'Save'. This saves the component. Update the form


![data context name resolved]({{ '/images/data-context-name-resolve.jpeg' | relative_url }})


**Figure 2. Requester's name dynamically resolved**

**You can use any of the components to display dynamic values, but don't use the 'Default Value' box in the component. This is because a default value is supposed to be static not dynamic i.e. the same value will always be loaded in that box and it appears before Formio does any processing.**

 
