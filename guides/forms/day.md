---
category: Forms
expires: 2020-12-31
order: 2
---
# 'Day' Component

## Introduction
You do not need to add a date/time component just for capturing the form submission date. This is implicitly captured when the user submits the form.
You should add a 'day' box to your form if you want to capture the day/month/year, for example an individual's date of birth, or the date on which an event occurred.
You should add a **[date/time]({{'/guides/forms/datetime/#datetime-component' | relative_url }})** box to your form if you will not only need to capture the day/month/year, but also the time in hrs/mins.

**After each change click 'Save' and 'Update Form' to save your changes.**

## Adding the 'Day' component

The form component 'Day' does not appear in the 'Basic' palette. In order to add the component 'Day' to your form you need to select 'Advanced' which appears below the list for the 'Basic' palette.

![Day component]({{ '/images/DayPalette.png' | relative_url }})

**Figure 1. Day Palette**

The 'Day' component will appear in edit/preview mode.

In the 'Label' box enter an instructive label e.g. 'Enter your date of birth'.

![Day label]({{ '/images/DayLabel.png' | relative_url }})

**Figure 2. Label Box**

You need to assign an API key to the field. If you do not create an API key, Formio will automatically generate a key based on the label. This should be avoided. The API key you assign is submitted to the 'back end' service. As described in API naming conventions, it is important to consult with your Data Team to make sure your API key follows conventions so that data can be retrieved and reported on correctly.

Change the API key to e.g. 'dateOfBirth' in the 'Property Name' box.

![Day API Key]({{ '/images/DayAPIKey.png' | relative_url }})

**Figure 3. Property Name Box with example API key**

## Naming your API keys
Use camelCase for all keys.
Don't put verbs in your camelCase.

Once you have filled in your label and changed the API, preview your form.
You will notice that the form previews with the American date format. This needs to be changed to UK date format.

To do this click on the 'Day' tab.
This will pop up a page with a 'Day First' option and a tick box. Click on the box. Once this box is selected the date will be changed to UK format, with the day first, followed by the month.

![Day First]({{ '/images/DayFirst.png' | relative_url }})

**Figure 4. 'Day First' tick box**


You will also notice that the form at this stage offers a drop down box for filling in the month. This does not follow GDS guildelines. GDS guildelines recommend using input boxes.
To change the drop-down box to an input box, click on 'Month' then 'Select' and change to 'Number'.

![Day Month Input]({{ '/images/DayMonthInput.png' | relative_url }})

**Figure 5. Selecting number to change to an input box**

Save your change by clicking the 'Save' button then 'Update Form', then 'Preview in UK Gov styling'. This opens a separate window and shows you the form, now with three input boxes for the date instead of drop down boxes.

## Changing your day component to pre-populate with a specific date
The most likely reason for wanting your date boxes to be prepopulated is because the user needs to record the date on which the form was filled in. It makes form-filling much easier for a user who has to record the date on every form they fill in.
You can create pre-populated date boxes after the point when you have dragged and dropped the 'Day' button into the container box. Follow all the steps until you have clicked on 'Number' to change from a drop-down box to an input box, then click the 'Data' tab at the top of the page. Click the 'Custom Default Value'. A list will appear of all the custom javascript that can be used. You need to use [moment](https://momentjs.com). This is the object you can use in the javascript you are going to be asked to write.

![Data tab]({{ '/images/DataTab.png' | relative_url }})

**Figure 6. Data tab**

![Day Custom Input]({{ '/images/CustomDefaultValue.png' | relative_url }})

**Figure 7. Custom Default Value panel**

Write the following javascript in the code box.

```js
value = moment().format("DD/MM/YYYY")
```


Click 'Save' then 'Update Form'.

## Validation

### Making a date mandatory
This is when the day, month, and year boxes on the form **must** be filled in. If this is the only type of validation your 'Day' field requires then use the following method.

In the 'Day Component' editor, click on the 'Validation' tab. Three check boxes will appear under the 'Validate On' box: 'Require Day', 'Require Month', 'Require Year'. Click the check boxes beside all of them. This will make all three boxes of the date mandatory so if any of them are left empty an error message will be returned.

![Mandatory day/month/year]({{ '/images/MandatoryDayFields.png' | relative_url }})

**Figure 8. Mandatory day/month/year**

The error message **must** be entered into the 'Custom Error Message' box. Otherwise Formio will auto-generate a message which will not be helpful to the user. The error message that appears when the date has not been completed should relate to that particular issue.

![Custom Error Message]({{ '/images/CustomErrorMessage.png' | relative_url }})

**Figure 9. Custom Error Message**


There are government guidelines for writing error messages. They can be found [here](https://design-system.service.gov.uk/components/error-message). They must be relevant, clear, and informative e.g. 'Passport issue date is required' (see Figure 8.).

### Making certain kinds of date invalid
If you have more than one validation requirement (even if it includes mandatory date) then use the 'Custom Validation' feature.
The simple date validation should now be enacted in the 'Custom Validation' feature, along with any further validation required. For example, if a passport issue date is requested, this date clearly cannot be in the future. In this instance the validation rule required is 'The date your passport was issued must be in the past', as well as 'Passport issue date is required'.

In order to do this, in the 'Validation' tab, select 'Custom Validation'. A drop down box will appear with a JavaScript editor box at the bottom. You need to enter the following JavaScript in the box.

```js
var passportIssueDate = moment(input, 'DD/MM/YYYY');
if (!passportIssueDate.isValid() || passportIssueDate.format('YYYY') === '0000') {
  valid = 'Passport issue date is required'
} else {
 valid = passportIssueDate.isBefore() ? true : 'The date your passport was issued must be in the past';
}
```

The above JavaScript includes validation of date as a mandatory field, as well as the 'date must be in the past' rule.


It is possible to create more complex rules in this way, but if you are not familiar with JavaScript you will need to consult with a developer.

### The error message
Your form will need to display an error message when the date has either not been completed or is in violation of the validation rule/s applied.


![PassporError]({{ '/images/PassportDate.png' | relative_url }})

**Figure 10. Passport issue date error message**

If you have customised the form validation then you need to write a custom error message in that JavaScript. In the JavaScript example it is possible to see the error messages between the single quotation marks (''). They appear after ```valid =``` This is where you will put your custom error message between single quotation marks e.g. ```valid = 'Passport issue date is required'```.

See [here](https://design-system.service.gov.uk/components/error-message) for guidelines on writing good error messages.


### 'Disable on Form Invalid' submit button
It is important to note Formio will disable the 'Submit' button if the form has not met all validation requirements. To switch off this feature click on the 'Edit' mode of the 'Submit' button. There is an option on the 'Submit' button edit preview page: 'Disable on Form Invalid'.

![Disable submit button]({{ '/images/DisableSubmitButton.png' | relative_url }})

**Figure 11. 'Disable on Form Invalid' button not selected**

If you 'untick' this box then the 'Submit' button will not be greyed out. It will remain clickable, but display an error message if the mandatory fields are not complete. For example:

![Error]({{ '/images/ValidationError.png' | relative_url }})

**Figure 12.  Validation error message on submit**
