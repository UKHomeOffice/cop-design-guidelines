---
category: Forms
expires: 2020-12-31
order: 6
---
# File component

## Introduction

Users are able to attach external files to their forms. This is facilitated by the form's file component. Every time a user attaches a file to the form that file is automatically stored in an external repository. The user can do this by using 'drag and drop' to move the file from an already open window or by clicking on 'browse' and then double-clicking the file.

## Adding a file component to your form

In the Form Builder select the 'Premium' tab. This opens a drop down box of options. Select the 'File' component and drag and drop to the 'Drag and Drop a form component' box.

![file palette]({{ '/images/file-palette.jpeg' | relative_url }})

**Figure 1. File component**

This will then open the 'File Component' modal.

![select storage option url]({{ '/images/select-storage-option-url.jpeg' | relative_url }})

**Figure 2. Select storage option - url**

You now need to configure the file component. It is possible to configure it to store files in many locations, but the only one you will ever use is the 'URL Storage' option.

You now need to give your file component an appropriate label in the 'Label' box e.g. supporting attachments. First select the 'File' tab, then click on the 'Storage' box. This presents a drop down list. From this select 'Url'. A 'Url' input box will appear.



You need to populate the 'Url' input box.

 ![url location]({{ '/images/url-location.jpeg' | relative_url }})

 **Figure 3. Url location**

 In the above example the expression only applies to COP UI. In eForms follow all the previous steps but just type '/files' in the 'Url' input box. All the remaining boxes on the 'File' tab can be ignored.


![eforms file input]({{ '/images/eforms-file-input.jpeg' | relative_url }})

**Figure 4. eForms file input**

If your user researcher has instructed you that adding an attachment needs to be mandatory on this form, you can do this by selecting the 'Validation' tab and ticking the 'Required' check box. You can also customise the error message that will be displayed if a user tries to submit the form without the mandatory attachment.

If you don't fill in the 'API' tab Formio will automatically generate a name which may not be sufficiently descriptive. Follow [guidelines on API naming conventions]({{'standards/naming-conventions/#general-guidance' | relative_url }}) when filling in the 'API' tab.

You do not need to fill in the 'Conditional', 'Logic', or ' Layout' tabs for most use cases.

When your component is completed click 'Save' and then select 'Update form' if it is an existing form or 'Create form' if it is a new form.
