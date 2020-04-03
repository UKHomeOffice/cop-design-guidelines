---
category: Workflow
expires: 2020-12-31
order: 2
---
# BPMN driven user forms

## Why use BPMN?

Whilst it is possible to put all the business processes  in the UI, using Formio, this results in long, complex forms composed of large amounts of conditional logic that then render really slowly and have big performance problems.

The way around this is to put most of the business logic in the BPMN rather than in the UI. The UI consists of a cover form  which then drives the presentation of subsequent forms via the BPMN. When the first form is submitted, the workflow service will then trigger the BPMN to check if there is another form to complete. If there is, this form will be displayed and the process repeated until there are no more forms that the user needs to fill in and the business process is completed.

Putting the logic in the BPMN in this way allows for the use of complicated rules and conditions without negatively impacting the performance of the user experience.

## When to use BPMN

Use BPMN when you have more than a few questions that need answering, or more than a few possible scenarios to accommodate. If the form begins to feel complex in the UI that is when to consider building a BPMN and putting the business logic there.

## How to use BPMN

First build the BPMN. This is an example workflow in BPMN for a car accident insurance form.

![example workflow]({{ '/images/example-workflow-car-insurance-claim-form.jpeg' | relative_url }})

**Figure 1. Workflow for car insurance claim form**


This will usually begin with a cover form that has been agreed with your user researcher.

![example workflow]({{ '/images/example-workflow-cover-form.jpeg' | relative_url }})

**Figure 2. Cover form for workflow**

Once you know what your cover form should include, then attach a decision model after the 'initial submission' in the BPMN. You can choose business rules or a user task to go straight after the 'initial submission'. For example in building a car accident insurance claim form the 'initial submission' form will capture the claimant's personal details, their insurance number, and whether another driver was involved. This first cover form will be submitted and drive the next form. If another driver was involved this would drive the next form 'other driver details'.

![example workflow]({{ '/images/example-workflow-was-there-another-driver-involved.jpeg' | relative_url }})

**Figure 3. example workflow for 'other driver details' form**

If another driver was not involved then the next form displayed would be 'description of incident' form. This form might end with the question 'Is there a police report?'.

![example workflow]({{ '/images/example-workflow-supply-police-incident-report.jpeg' | relative_url }})

**Figure 3. example workflow for 'police report' form**


If the answer to this was 'no' the next form presented would be the 'vehicle details' form. If the answer was 'yes' the 'police report' form would be displayed, and then the 'vehicle details' form once the 'police report' form was completed.

![example workflow]({{ '/images/example-workflow-mechanics-report.jpeg' | relative_url }})

**Figure 4. example workflow for 'mechanic's report' form**


The 'vehicle details' form might end with the question 'is there a mechanic's report on the damage?'. If the answer to this was 'yes' the 'mechanic's report' form would appear if the answer was 'no', the form submission would be completed.

## Constraints


* The BPMN has to be user-task dependent. Forms are displayed when there is a user task associated with that form.

* User tasks cannot be asynchronous. The tasks have to be synchronous and the workflow keeps moving forward through the tasks to a 'safe point' or the end of the workflow.

* The workflow should return one single task at a time, not a collection. If you need to display a collection of forms, then show one at a time and use a loop construct to show the other forms one after another.
