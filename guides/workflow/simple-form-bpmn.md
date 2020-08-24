---
category: Workflow
expires: 2020-12-31
order: 4
---
# Building a form and BPMN, and deploying to COP and eForms

## Introduction

This is a guide to how to build simple forms in Form Builder, linking those forms to a BPMN, and then uploading the BPMN to COP, using the Camunda modeller.

The first form is a 'request' form, in this example a request for holiday or time off. The second form is an 'approval' form. It displays the data submitted in the first form, and allows the approver to approve or deny the holiday request.

This guide will explain not only how to build a form, but how to associate it to a BPMN, and then deploy that to COP.

## Building the forms

You need to decide which components you need on your form. For this first example, 'holiday request' form, there are three components:

* Disabled text box pre-populated with the user's given and family name.

* Day component capturing the holiday start date. This will be labelled 'from'.

* Day component capturing the holiday end date. This will be labelled 'to'.


![holiday request form]({{ '/images/holiday-request-form-edit.jpeg' | relative_url }})

**Figure 1. form builder edit mode**

The 'Requester' box is pre-populated with the user's given name and family name as a result of data context interpolation i.e. once a user is logged in, the UI will pre-populate any boxes with information attached to that login: given name, surname, and email. Once a user has signed onto their shift, this information might also include shift details and roles. For more information on data context interpolation see [here]({{'/guides/forms/data-context' | relative_url }}).

Information on how to add the day components for the 'from' and 'to' inputs [here]({{'/guides/forms/day' | relative_url }}).

When you preview your form in GovUK styling it will look like this:



![holiday request form GDS preview]({{ '/images/holiday-request-form-preview.jpeg' | relative_url }})

**Figure 2. holiday request form in GDS preview**

The 'holiday request workflow' has been triggered by the 'holiday request' form. Once the 'holiday request' form has been submitted, the workflow transitions to the approval task, which will present the 'approval form' to the 'approver'. The 'approval form' must contain the following components:

* details of person who requested holiday: 'Requester'.

* 'from' and 'to' dates of the holiday period requested.

* approval 'yes' or 'no' radio buttons.

![approve holiday request form]({{ '/images/approve-holiday-request-preview.jpeg' | relative_url }})

**Figure 3. approval form in form builder edit mode**

In order to pre-populate the 'Requester' field you need to open the component in Form Builder edit mode and select 'Calculated Value' panel. In the JavaScript section add this:

```
value = data.processContext.holidayRequest.requester;
```
The above code loads the variable 'holidayRequest' from the process instance, and then the property 'requester'.

In order to prepopulate the 'from' box on the 'approve holiday request' form, follow the same process. In Form Builder edit mode for the 'from' component select the 'Calculated Value' panel. In the Javascript section enter this:

```
value = data.processContext.holidayRequest.fromDate;
```
The above code loads the variable 'holidayRequest' from the process instance, and then the property 'fromDate'. This same principle is applied to the pre-populated 'to' box. Use the same code but replace 'fromDate' with 'toDate'.

The form will be presented to the 'approver' in a pre-populated state. See Figure 4.

![approve holiday request form gds preview]({{ '/images/approve-holiday-request-gds-preview.jpeg' | relative_url }})

**Figure 4. approval form in GDS preview**

## Building the simple workflow for 'holiday request'

The workflow will be built in the Camunda modeller. Once you open the modeller you will be presented with this option 'Create a BPMN diagram or Create a DMN diagram'. Select 'BPMN diagram'. See Figure 4.

![camunda modeller options]({{ '/images/camunda-modeller-options.jpeg' | relative_url }})

**Figure 4. camunda modeller options**

You will then be presented with the process properties panel. See Figure 5. It is important you provide the role in the 'Candidate Starter Groups' or you won't see it appear in the UI. Once the process has been started with the candidate group if you don't have that role you will never see the process. So for example if your manager or approver doesn't have the same role they will not see the approval task.


![process properties panel]({{ '/images/process-properties-panel.jpeg' | relative_url }})

**Figure 5. process properties panel with properties completed**

### How to add the first form - 'holiday request'

The modeller automatically provides a start event and the properties panel. Select the start event notation (Fig. 6.)

![start event notation]({{ '/images/start-event-notation.jpeg' | relative_url }})

**Figure 6. start event notation**

Next fill in the 'Id' and 'Name' boxes. The 'Id' is useful for writing unit tests because you can reference activities by the id. If it's an autogenerated id it's difficult to remember or know what it is. The 'Name' box should correspond to the label on the start node. Click on the 'Forms' tab. Write the name of the form in the 'Form Key' box. This will be used by the UI to load the form.


### How to add the 'Approver form'


Next create the 'approval task'. Select 'Create Task' icon on the palette or click on the start node and then select the 'Append Task' icon. Once you have clicked on it, type the  task name in the 'Name' box. **Always use a verb. A task should always be an action.** In this example use 'Approve holiday request'. This task then needs to be converted to a specific type. If you want to present the 'approve holiday request' form to the 'approver' then you need to convert the task to a 'user task'. To do this click on the 'change type' spanner icon. A drop down list will appear. Select 'User Task' (see Figure 7.)

![change type drop down list]({{ '/images/change-type-drop-down-list.jpeg' | relative_url }})

**Figure 7. 'Change type' drop down list**

The tab for the 'User task' appears. In the 'Id' box fill in a descriptive Id that will be easy to remember. In the 'Details' section there are three pieces of information that need to be completed. First you need to decide who the task will go to. You can assign the form to an individual, or a candidate group/team. The individual can be static, i.e. a specifically identified individual, or dynamic, i.e. a value that's derived from the form or loaded from an external source.

If you want to assign it to an individual, but one that is dynamically provided by the form, then input the following variable expression:

 ```
 ${S(holidayRequest).prop('approverEmail').stringValue()}
 ```

 This expression:

 ```
 ${S(holidayRequest)
   ```
 references the variable 'holidayRequest' which contains the data that's been submitted.

```
.prop('approverEmail').stringValue()}
```

Inside the object is a property called 'approverEmail' and you want the value of that i.e. the email address of the individual who needs to approve the form. [For more info on spin](https://docs.camunda.org/manual/latest/reference/spin/json/01-reading-json/)

The task can also be assigned to a group. In the 'Candidate Groups' box use almost the same expression except the property will be e.g. 'approverTeam' rather than 'approverEmail':

```
${S(holidayRequest).prop('approverTeam').stringValue()}
```
The 'approver team' can be a comma separated string. Camunda will automatically convert that into a list.

If your task requires a due date, for example if you want to use due dates to order tasks in the COP task list, then fill in the due date box. If you use a dynamic expression then the due date will be set when the task is created. This is the dynamic expression to use:

```
${now()}
```
You also need to fill a description in the 'Documentation' box so that the 'Task View' will explain the nature of the task.

![documentation box]({{ '/images/documentation-box.png' | relative_url }})

**Figure 8. 'Documentation' box**

Now we want to present a form to the 'approver'. Do this by clicking on the 'Form' tab. Copy the name of the 'approve holiday request' form and put it in the 'Form Key' box.

![approve request form key]({{ '/images/approve-holiday-request-form-key.jpeg' | relative_url }})

**Figure 9. approve request form key**

Now finish the BPMN by attaching an 'End Event'. Follow the same process. First give it an 'Id'. Give it a name to represent that the process is complete e.g. 'Request approved'. The BPMN is now complete. Your next task is to deploy it to COP.

### How to deploy the BPMN to COP and eForms

The Camunda workflow engine underpins both COP and eForms, but the COP engine is highly customised to suit Border Force requirements, whereas eForms is more general. The deployment process for both is the same, the only difference is the URL.

The following instructions apply both to deployment to COP and eForms with two exceptions: only certain people are allowed to deploy to eForms, and a different URL will be provided when it comes to eForms.

Only certain roles e.g. tech lead, have clearance to deploy to eForms. This encourages developers to fully test their BPMN before deploying it, rather than deploying it and testing it afterwards.

Save the file in the appropriate repository. (Please consult with your developer/tech arch. If you are testing this locally you can save it in your local directory.) Then click on the arrow button at the end of the toolbar 'Deploy Current Diagram'. Give the diagram a name e.g. 'Holiday Request', then the url of the workflow engine that's running (See Fig. 10).

![deploy to cop]({{ '/images/deploy-to-cop.jpeg' | relative_url }})

**Figure 10. Deploy to COP**

REST endpoint for COP:
```
<COP_URL>/rest/camunda/deployment/create
```
REST endpoint for eForms:
```
<eForms_URL>/camunda/engine-rest/deployment/create
```

**Consult your tech lead for the appropriate COP_URL or eForms_URL**

Next you need a 'bearer token'. A 'bearer token' is a form of access token, that encodes information about the individual deploying the BPMN.
The 'bearer token' only lasts for a limited amount of time so you need to generate one every time you deploy your BPMN to COP and eForms.

You can download a tool called [postman](https://www.postman.com). Documentation on how to use postman can be found [here](https://learning.postman.com). This [link](https://learning.postman.com/docs/sending-requests/authorization/#oauth-20tells) shows you how to get a 'bearer token'. In the form 'Get New Access Token' you need to fill in 'Token Name','Callback URL', 'Auth URL', 'Access Token URL', and a 'Client ID'.

* 'Token Name' - this can be anything, as it only lasts for a short period of time.

* 'Callback URL' - this can be your local machine, so fill in 'http://localhost:8080'

* 'Auth URL' - your DevOps will provide you with the 'Auth URL', you then need to input it into the 'Auth URL' box and add '/auth'.

* 'Access Token URL' - will be available from your Tech Lead or DevOps engineer. Input it into the 'Access Token URL' box and add '/token'.

* 'Client ID' - will either be 'eforms' or 'www'(for COP).

![get new access token]({{ '/images/get-new-access-token.jpeg' | relative_url }})

**Figure 11. Get New Access Token form**

Once you have filled in all that information click 'Request Token'. You will then be redirected to 'Keycloak SSO'. Next you will be prompted to authenticate using your credentials.

Once you have authenticated you will be presented with the 'Manage Access Tokens' box. Copy the access token value. Next, go to the BPMN modeller and paste that token into the 'Token' box.

![bearer token]({{ '/images/bearer-token-input.jpeg' | relative_url }})

**Figure 12. Bearer token input**

Then you click the 'Deploy button'.

Once the BPMN is deployed to eForms you should see this:

![form successfully deployed to eForms]({{ '/images/successful-deploy-to-eforms.jpeg' | relative_url }})

**Figure 13. Form successfully deployed to eForms**

Once the BPMN is deployed to COP you should see this:


![holiday request form]({{ '/images/holiday-request-form-cop.jpeg' | relative_url }})

**Figure 14. Holiday request form in COP**


Once the form has been submitted, the 'approver' will see an 'approval task':


![approver task]({{ '/images/approver-task.jpeg' | relative_url }})

**Figure 15. Approval task**

<br/>
Once the 'approver' clicks on the 'Action' button they will be presented with the 'approval form':

![approver task cop]({{ '/images/approval-form-cop.jpeg' | relative_url }})

**Figure 16. Approval form**

<br/>
Once the 'approver' has completed the 'approval form' the task is submitted and the workflow is finished. The 'approval task' disappears from the 'approvers' work queue.
