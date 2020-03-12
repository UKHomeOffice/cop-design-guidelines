# Tooltip Title

Having filled in your [tooltip explanation](/design-guidelines/guides/tooltips/tooltip-explanation) you now need to create a title for the tooltip. If you do not create a custom title for the tooltip it will display with the default `Help`. It is recommended that you use a custom title rather than resorting to the default.

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

[Tooltip overview](/design-guidelines/guides/tooltips/overview)

[How to add a tooltip](/design-guidelines/guides/tooltips/how-to-add-a-tooltip)

[Tooltip explanation](/design-guidelines/guides/tooltips/tooltip-explanation)

[Your completed tooltip](/design-guidelines/guides/tooltips/your-completed-tooltip)
