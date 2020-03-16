---
category: Forms
expires: 2020-12-31
order: 2
---
# 'Day' and 'Date and Time' Component

## 'Day' or 'Date and Time'?
You do not need to add a date/time component just for capturing the form submission date. This is implicitly captured when the user submits the form.
You should add a 'day' box to your form if you want to capture the day/month/year for example an individual's date of birth, or the date on which an event occurred.
You should add a 'date and time' box to your form if you will not only need to capture the day/month/year, but also the time in hrs/mins.

## Day

The form component 'day' does not appear in the 'Basic' palette. In order to add the component 'day' to your form you need to select 'Advanced' which appears below the list for the 'Basic' palette.

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

Save your change by clicking the 'save' button then 'update form', then 'Preview in UK Gov styling'. This opens a separate window and shows you the form, now with three input boxes for the date instead of drop down boxes.

## Changing your day component to pre-populate with a specific date
The most likely reason for wanting your date boxes to be prepopulated is because they need to record the date on which they were filled in. It makes form-filling much easier for a user who has to record the date on every form they fill in.
You can create pre-populated date boxes after the point when you have dragged and dropped the 'day' button into the container box. Follow all the steps until you have clicked on 'number' to change from a drop-down box to an input box, then click the 'data' tab at the top of the page. Click the 'custom default value'. A list will appear of all the custom javascript that can be used. You need to use 'moment'. This is the object you can use in the piece of javascript you are going to be asked to write.

![Day Custom Input]({{ '/images/DayCustomInput.png' | relative_url }})

**Figure 6. Custom Default Value panel**

Write the following javascript in the code box.

```js
value = moment().format("DD/MM/YYYY")
```


Click 'save' then 'update form'.
