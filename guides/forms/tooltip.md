---
category: Forms
expires: 2020-12-31
order: 3
---
# To Tooltip Or Not To Tooltip?

Tooltips are a very important component of your form. They can significantly impact the user's ability to use a form as they provide explanations of what is required by a field, as well as guidance on what to do if the required information is not available to the user.

**Nevertheless it is not a good idea to litter a form with tooltips.**

When you are considering adding a tooltip ask yourself if your user really needs one. In accordance with GDS guidelines a tooltip should provide information that only some users may require. To clarify, if you are considering adding information that every user will need then it should appear permanently on the form as 'description' text. On the other hand, if the information you are considering adding is so obvious that only a very few, or perhaps none, of the users will need it then do not add it.

You need to make a judgement about whether a significant number of users will need a field to be explained in greater detail. You can ask a user researcher for advice if you are unsure, and it is a good idea to ask your user researcher to pay particular attention to tooltips when your form is tested with users.


## How To Add A Tooltip

There is only one acceptable way to add a tooltip, and that is by selecting the 'edit' feature on the form component in the builder, and opening the edit modal.

![Edit component](https://user-images.githubusercontent.com/290639/76080234-5b552680-5f9e-11ea-98d5-a7ebb35c176b.png)

**Figure 1. Edit component**

Then click on the tooltip text box.

![Tooltip box](https://user-images.githubusercontent.com/61820359/76084678-51d0bc00-5fa8-11ea-8871-cea57f9cd2c6.png)

**Figure 2. Tooltip text component**

It is never acceptable to add html content via the Form Builder palette to create a tooltip for a field. This is because you will be adding unnecessary code which may cause the form to render too slowly. Also the more code there is, the more opportunities there are for errors.

**As a general rule you should not be editing code in JSON in order to build forms. One exception to this is adding your [tooltip title](/design-guidelines/guides/forms/tooltip/#tooltip-title). If you find you are editing JSON ask yourself if there is actually a feature for this in builder that you are not using. If there is no existing feature consult with Forms Platform development team.**

The tooltip text box is where you will enter your [tooltip explanation](/design-guidelines/guides/forms/tooltip/#tooltip-explanation).



## Tooltip Explanation

The tooltip box is where you will add your tooltip explanation. This is the sentence or brief paragraph that will appear when the user clicks on the [tooltip title](/design-guidelines/guides/forms/tooltip/#tooltip-title). The information provided in the explanation is very context-specific. You may need to consult with your content designer or user researcher. However there are some general rules. The tooltip explanation should clarify for the user what is required in the associated field. It is alright to use specialist terminology your user will understand, but keep the explanation as simple and clear as possible. The explanation can be typed directly into the ``Tooltip`` box in the builder without the need to use html markup.

![add-tooltip-description-modal](https://user-images.githubusercontent.com/61820359/76081828-e08e0a80-5fa1-11ea-8a7e-9b4b04054f40.png)


**Figure 3. Tooltip explanation without any markup**

If you need to use html markup, for example to create a summary list or a table, you can, just copy and paste or enter your html into the box.

![Markup tooltip](https://user-images.githubusercontent.com/61820359/76082056-6742e780-5fa2-11ea-95af-81a8e304b882.png)


**Figure 4. Tooltip explanation with markup**


## Tooltip Title

Having filled in your [tooltip explanation](/design-guidelines/guides/forms/tooltip/#tooltip-explanation) you now need to create a title for the tooltip. If you do not create a custom title for the tooltip it will display with the default `Help`. It is recommended that you use a custom title rather than resorting to the default.

To do this go to the field in the builder. Click the `Edit JSON` button.

![edit-json-toolbar](https://user-images.githubusercontent.com/61820359/76082751-f7cdf780-5fa3-11ea-939a-06e20325a428.png)

**Figure 5. Edit JSON button**

This opens the form in JSON. Add the following key value `"tooltipTitle" : "",`. Your title goes between the quotation marks. There is no specific location for this line. You can put it anywhere in the root of the component schema.


![Tooltip JSON code](https://user-images.githubusercontent.com/61820359/76083009-82165b80-5fa4-11ea-94c5-81d5c7e8aa0e.png)

**Figure 6. Tooltip JSON configuration**

A 'tooltip title' corresponds to GDS 'details summary' text. It should explain clearly what the tooltip will help the user with. Ideally it should begin with the words "Help with...". Use clear, simple language.

![Title](https://user-images.githubusercontent.com/61820359/76083343-4d56d400-5fa5-11ea-8293-3f74a012b534.png)

**Figure 7. Tooltip title**

**In the near future the Forms Platform will add a tooltip title text box in the component edit modal so plain text can be typed straight into that box to create a title.**


## Your Completed Tooltip

Your completed tooltip should appear like this. A clear title which expands when clicked to show a relevant and clear explanation.

![gds-tooltip-expanded](https://user-images.githubusercontent.com/61820359/76084799-aaa05480-5fa8-11ea-9820-332125e06d8b.png)

**Figure 8. Completed tooltip**
