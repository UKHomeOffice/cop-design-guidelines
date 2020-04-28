---
category: Forms
expires: 2020-12-31
order: 5
---
# How to Create Dynamic Select Boxes, Radio Buttons, and Static Select Lists

## Introduction
This section explains how to create dynamic values for radio buttons, select boxes, and static select components (dropdown boxes), based on conditional data. The way in which you hide or display options for all these selection components is the same. It can be applied to radio buttons, select boxes, and static select lists. If you have a static select list you can generate dynamic values in the drop down box. Radio buttons only allow one option to be selected, whereas check boxes and static select lists allow multiple selections.

We will use an example form that has a select box component, and some of the values will be dynamically displayed based on the user role. In the example given below the form will be a Customer Services department complaint form. In this scenario a Customer Services Team Leader may be filling in the form, with the authority to decide whether the complaint should be investigated or not. A Team Member will only be able to record the complaint.


## How to add dynamic options to your 'Select boxes'

After opening your form builder and giving it a 'Title' and 'Path' in the normal way, choose 'Select boxes' from the 'Basic' menu and drag and drop it. The 'Select boxes' edit modal will then be displayed. Give it a 'Label'. Our label is 'Choose one or more complaint actions'.
There will be two default options displayed whether the user filling in the form is a Team Lead or Team Member. Select the 'Data' tab of the edit modal. In the 'Label' box enter a label that will be displayed next to the 'check box' on the form. Next provide a 'Value' for the first 'check box'.  This is 'Recorded'. To add a second 'check box' click the 'Add Another' button. These check boxes will always be displayed, regardless of whether a Team Lead or Team Member is filling in the form.  

**Whenever your form will have more than one option, always choose 'select boxes', even if in certain instances only one box will be displayed at first.**


![Select buttons]({{ '/images/edit-select-add-options.jpeg' | relative_url }})

**Figure 1. Adding a 'check box' (using 'Select Boxes Component')**

### Defining conditional logic

In the API tab give it a 'Property Name' e.g. 'complaintActions'. Next go to the 'Logic' tab. Give it a 'Logic Name' e.g. 'dynamicCheckOptionsLogic'. Next code in the 'Trigger'. This tells Formio when to display the additional options. Code in the Trigger by filling in the Javascript 'Text Area' in the 'Trigger' component. Your code should return a boolean result. Example code is below:

```
var roles =  data.user ? data.user.roles: [];
result = _.find(roles, (r) => r === 'team-lead');
```
The above code checks whether the user filling in the form has the role 'team-lead'. The expression should return 'true' or 'false'. If it's true, then the additional options will be displayed.

### Defining the dynamic values

Now you have defined the 'Trigger', you need to configure Formio to perform an action based on the 'Trigger'. In our example we want to add two additional options to the 'Select boxes'. Click on 'Add action'. You will be presented with the 'Actions' panel. Fill in the 'Action Name' box e.g. 'addTeamLeadOptions'. Choose the type of actions you want to perform. Select from the 'Type' drop down box the option 'Value'. In the 'Value (Javascript)' box write the following code, which defines the dynamic values:

```
component.values = _.concat(component.values, [
   {
    "label" : "Approve",
    "value" : "approve"
   },
   {
   "label": "Rejected",
   "value": "rejected"
   }
  ]);
  ```



Select 'Save Action' button, then 'Save Logic' button, **in this order**. Then select 'Save' button, to save the component.

![Select buttons]({{ '/images/dynamic-options-employee.jpeg' | relative_url }})

**Figure 2. Default Checkboxes displayed for both Team Manager and Team Member**

![Select buttons]({{ '/images/dynamic-options-team-lead.jpeg' | relative_url }})

**Figure 3. Checkboxes displayed for only Team Manager**


## How to add dynamic options to 'Radio' buttons

Radio buttons only allow one option to be selected. In our example Customer Services form the default choices will be 'Upheld' or 'Denied' and the dynamic option is 'Further investigation required'.

After opening your form builder and giving it a 'Title' and 'Path' in the normal way, choose 'Radio component' from the 'Basic' palette. Follow the same steps as for 'Checkboxes', filling in a 'Label' and 'Value'.

![Select buttons]({{ '/images/radio-buttons-label-value.jpeg' | relative_url }})

**Figure 4. 'Label' and 'Value' components for 'Radio buttons'**

Follow the same steps in the 'Defining conditional logic' in the ['Select boxes' section.]({{'guides/forms/dynamic-elements/#defining-conditional-logic'| relative_url }})


![Select buttons]({{ '/images/radio-button-default.jpeg' | relative_url }})

**Figure 5. Default 'Radio' buttons on the Team Member form**



![Select buttons]({{ '/images/radio-button-upheld-denied.jpeg' | relative_url }})

**Figure 6. 'Radio' buttons with 'Further investigation required' dynamic button on the Team Lead form**


## How to add dynamic options to a Static Select List

Follow the same principle as above, but when you come to fill in dynamic options in the 'Actions' panel, 'Value (JavaScript)' text box use the following:

```
component.data.values = _.concat(component.data.values, [
   {
    "label" : "Further investigation required",
    "value" : "furtherInvestigation"
   }
  ])
  ```
  
