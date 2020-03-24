---
category: Forms
expires: 2020-12-31
order: 3
---
# 'Date/Time' Component

## Introduction
There are particular user cases when a 'date/time' component is required on a form. This captures not only the year, month, and day of an event but the time as well, in hours and minutes. An example of a situation in which this may be required is the administration of medicine.
You should add a **[day]({{'/guides/forms/day/#day-component' | relative_url }})** box to your form if you want to capture the day/month/year, for example an individual's date of birth, or the date on which an event occurred. You should add a 'date/time' box to your form if you will not only need to capture the day/month/year, but also the time in hrs/mins.
The **'date/time'** component displays year, month, day, hrs, minutes.

**After each change, click 'Save', then 'Update Form' to save your changes.**

## Adding the 'Date/Time' component

The form component 'Date/Time' does not appear in the 'Basic' palette. In order to add it to your form you need to select 'Advanced' which appears below the list for the 'Basic' palette.

![Day component]({{ '/images/DayPalette.png' | relative_url }})

**Figure 1. Day Palette**

Select the 'Date/Time' button. The 'Date/Time' component will appear in edit/preview mode.

In the 'Label' box enter an instructive label e.g. 'Enter date and time at which medication administered'.

![Day label]({{ '/images/datetimelabel.png' | relative_url }})

**Figure 2. Label Box**

You need to assign an API key to the field. If you do not create an API key, Formio will automatically generate a key based on the label. This should be avoided. The API key you assign is submitted to the 'back end' service. As described in API naming conventions, it is important to consult with your Data Team to make sure your API key follows conventions so that data can be retrieved and reported on correctly.

Change the API key to e.g. 'timeMedicineAdministered' in the 'Property Name' box.

![DateTime API Key]({{ '/images/datetimeAPI.png' | relative_url }})

**Figure 3. Property Name Box with example API key**

## Naming your API keys
Use camelCase for all keys.
Don't put verbs in your camelCase.


Save your change by clicking the 'Save' button then 'Update Form', then 'Preview in UK Gov styling'. This opens a separate window and shows you the form, now with three input boxes for the date and two input boxes for the time.

![DateTime API Key]({{ '/images/DateTimeGDSPreview.png' | relative_url }})

**Figure 4. date and time input boxes in UK Gov preview**

## Changing your date/time component to pre-populate with a specific date
The most likely reason for wanting your date/time boxes to be prepopulated is because the user needs to record the date and time on which the form was filled in. It makes form-filling much easier for a user who has to record the date and time on every form they fill in.

You can create pre-populated date/time boxes after the point when you have dragged and dropped the 'date/time' component into the container box.
Click the 'Data' tab at the top of the page. Click the 'Custom Default Value'. A list will appear of all the custom javascript that can be used. You need to use [moment](https://momentjs.com). This is the object you can use in the javascript you are going to be asked to write.

![Data tab]({{ '/images/DataTab.png' | relative_url }})

**Figure 5. Data tab**

![Day Custom Input]({{ '/images/CustomDefaultValue.png' | relative_url }})

**Figure 6. Custom Default Value panel**

Write the following javascript in the code box.

```js
value = moment()
```

This fills in the date and time at which the form was loaded and this will persist until the form is submitted.

Click 'Save' then 'Update Form'.

## Validation

### Making a date mandatory
This is when the date and time boxes on the form **must** be filled in, before the form can be submitted. If this is the only type of validation your 'Date/Time' field requires then use the following method.

In the 'Date/Time Component' editor, click on the 'Validation' tab. A 'Required' check box will appear. Click the check box. This will make filling all the date and time boxes mandatory, so if any of them are left empty an error message will be returned.

![Mandatory date/time]({{ '/images/DateTimeValidationRequired.png' | relative_url }})

**Figure 7. Mandatory date/time**

The error message **must** be entered into the 'Custom Error Message' box. Otherwise Formio will auto-generate a message which will not be helpful to the user. The error message that appears when the date has not been completed should relate to that particular issue.

![Custom Error Message]({{ '/images/DateTimecustomerrormessage.png' | relative_url }})

**Figure 8. Custom Error Message**


There are government guidelines for writing error messages. They can be found [here](https://design-system.service.gov.uk/components/error-message). They must be relevant, clear, and informative e.g. 'Date and time of administering medicine is required' (see Figure 8.).

### Making certain kinds of date invalid
If you have more than one validation requirement (even if it includes mandatory date) then use the 'Custom Validation' feature.
The simple date validation should now be enacted in the 'Custom Validation' feature, along with any further validation required. For example, if a record of the date and time a medicine was administered is required, this cannot be in the future. In this instance the validation rule required is 'The date and time medicine was administered must be now, or in the past'  as well as 'Medicine administration date and time is required'.

In order to do this, in the 'Validation' tab, select 'Custom Validation'. A drop down box will appear with a JavaScript editor box at the bottom. You need to enter the following JavaScript in the box.

```js
var medicineAdminstrationDate = moment(input);
if (!medicineAdminstrationDate.isValid()) {
 valid = 'Medicine administration date is required';
} else {
 valid = medicineAdminstrationDate.isBefore() || medicineAdminstrationDate.isSame()  ? true : 'The medicine administration date cannot be in the future';
}
}
```

The above JavaScript includes validation of date and time as a mandatory field, as well as the 'date and time must be now or in the past' rule.


It is possible to create more complex rules in this way, but if you are not familiar with JavaScript you will need to consult with a developer.

### The error message
Your form will need to display an error message when the date has either not been completed or is in violation of the validation rule/s applied.


![AdministratingMedicineError]({{ '/images/DateTimeValidationErrorMessage.png' | relative_url }})

**Figure 9. Medicine Administration date and time error message**

If you have customised the form validation then you need to write a custom error message in that JavaScript. In the JavaScript example it is possible to see the error messages between the single quotation marks (''). They appear after ```valid =``` This is where you will put your custom error message between single quotation marks e.g. ```valid = 'The medicine administration date cannot be in the future'```.

See [here](https://design-system.service.gov.uk/components/error-message) for guidelines on writing good error messages.


### 'Disable on Form Invalid' submit button
It is important to note Formio will disable the 'Submit' button if the form has not met all validation requirements. To switch off this feature click on the 'Edit' mode of the 'Submit' button. There is an option on the 'Submit' button edit preview page: 'Disable on Form Invalid'.

![Disable submit button]({{ '/images/DisableSubmitButton.png' | relative_url }})

**Figure 10. 'Disable on Form Invalid' button not selected**

If you 'untick' this box then the 'Submit' button will not be greyed out. It will remain clickable, but display an error message if the mandatory fields are not complete. For example:

![Error]({{ '/images/DateTimeValidationSubmitErrorMessage.png' | relative_url }})

**Figure 11.  Validation error message on submit**
