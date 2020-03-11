There is only one acceptable way to add a tooltip, and that is by selecting the 'edit' feature on the form component in the builder, and opening the edit modal.

![Edit component](https://user-images.githubusercontent.com/290639/76080234-5b552680-5f9e-11ea-98d5-a7ebb35c176b.png)

**Figure 1. Edit component**

Then click on the tooltip text box.

![Tooltip box](https://user-images.githubusercontent.com/61820359/76084678-51d0bc00-5fa8-11ea-8871-cea57f9cd2c6.png)

**Figure 2. Tooltip text component**

It is never acceptable to add html content via the Form Builder palette to create a tooltip for a field. This is because you will be adding unnecessary code which may cause the form to render too slowly. Also the more code there is, the more opportunities there are for errors.

**As a general rule you should not be editing code in JSON in order to build forms. One exception to this is adding your [tooltip title](/design-guidelines/tooltips/Tooltips:-The-tooltip-title). If you find you are editing JSON ask yourself if there is actually a feature for this in builder that you are not using. If there is no existing feature consult with Forms Platform development team.**

The tooltip text box is where you will enter your [tooltip explanation](/Tooltips:-The-tooltip-explanation).
