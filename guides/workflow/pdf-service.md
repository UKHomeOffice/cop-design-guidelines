---
category: Workflow
expires: 2020-12-31
order: 6
---
# Generating a PDF of a Form

## Introduction

Certain types of form, when completed by a user, generate a PDF of the specific instance of the filled in form, which is then uploaded to Amazon S3. This grew out of the need to disseminate copies of forms via email to specific individuals or teams.
There is a ready-made, reusable BPMN sub-process that can be incorporated into the BPMN for any form.

## PDF generation process

The main active components are BPMN, Form API Service, Redis, Puppeteer, and the PDF Service. The initiator is the reusable BPMN sub-process. This sends a request to the Form API Service for PDF generation. This generates a job that is put onto Redis (a persistent cache), then the PDF Service is notified of the job. The PDF service then executes the job by running Puppeteer, which opens a headless browser and renders the form with the submission data. A PDF is then generated and if it is successful the PDF Service will upload the document to Amazon S3.
Once the document has been successfully uploaded to S3 the PDF Service will notify the 'caller/initiator' of the location of the PDF document. A Webhook URL has been provided for the PDF Service to notify the 'caller/initiator'.

### How to configure reusable BPMN sub-process

![generate pdf bpmn]({{ '/images/generate-pdf-bpmn.jpeg' | relative_url }})
