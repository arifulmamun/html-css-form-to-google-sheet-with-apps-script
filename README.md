# html-css-form-to-google-sheet-with-apps-script
* No redirect success message url (Stop google apps script default behavior), Success message populated on the page after successful send click event and it last long only 15 seconds.

**STEP:1**
> Go go [Google Sheet](https://sheets.google.com/)
> Create a Sheet and name it e.g, My Email List.
> Create a Sheet Header exact below image since it is case sensitive part matching apps script code :thinking:


<img width="500" alt="sheet" src="https://github.com/user-attachments/assets/61b8f2ca-c0a8-460e-97ea-c8adcc5f4ead">

**STEP:2**
> Create Apps Sript for the Sheet & name it.


<img width="500" alt="apps script" src="https://github.com/user-attachments/assets/ddc8886e-7304-44f9-8713-afb83e87e3e9">

**STEP:3**
> Find out left code.js file replace or paste below code

```js
const sheetName = 'Sheet1'
const scriptProp = PropertiesService.getScriptProperties()

function initialSetup () {
  const activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
  scriptProp.setProperty('key', activeSpreadsheet.getId())
}

function doPost(e) {
  const sheetName = 'Sheet1';
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(sheetName);

  const data = e.parameter;
  const name = data.name;
  const email = data.email;
  const question = data.question;
  const date = new Date();

  const lastRow = sheet.getLastRow();
  const nextRow = lastRow + 1;

  const newRow = [date, name, email, question];

  try {
    sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow]);
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    Logger.log('Error:', error);
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': error.message }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

**STEP:4**
> Save it and then Run button.This time you have to ask permission and click unsafe. Now you see in the script editor console **Execution log**

![save it](https://github.com/user-attachments/assets/e9ae206a-6440-4c94-a144-3a36f9199c9c)



**STEP:5**
> Find out left side panel Triggers menu click it and **Add Triggers**
> A window appear so fill this: **doPost**, **Head**, **from Spreedsheet**, **on Form submit** and **Save** click.

<img width="500" alt="dopost" src="https://github.com/user-attachments/assets/ed6036c6-c451-4d24-aa3e-ba5aef465ac6">


