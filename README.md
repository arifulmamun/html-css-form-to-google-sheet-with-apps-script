# html-css-form-to-google-sheet-with-apps-script
* No redirect success message url (Stop google apps script default behavior), Success message populated on the page after successful send click event and it last long only 15 seconds.

**STEP:1**
> Go go [Google Sheet](https://sheets.google.com/)
> Create a Sheet and name it e.g, My Email List.
> Create a Sheet Header exact below image since it is case sensitive part matching apps script code :thinking:

![sheet](https://github.com/user-attachments/assets/61b8f2ca-c0a8-460e-97ea-c8adcc5f4ead)

**STEP:2**
> Create Apps Sript for the Sheet & name it.

![appsscript create](https://github.com/user-attachments/assets/ddc8886e-7304-44f9-8713-afb83e87e3e9)

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
> Click Deploy then New Deployment.

![deployment](https://github.com/user-attachments/assets/4fee8284-620e-462e-8882-30a010a1e1b0)


