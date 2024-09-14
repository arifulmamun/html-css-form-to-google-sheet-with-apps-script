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

**STEP:6**
> Apps Script Editor find right up corner **Deploy** then **New Deployment**
> New window appear so find **Configuration** gear icon click it then fill up as below:
* Description: Mailing List Form for and example you can define what you want but should be small name to recall it.
* Web app → Execute As: Me
* Web app → Who has access: Anyone
* Save it.
> **IMPORTANT** At this stage :warning: copy url it will be use your html css form so take care it.

**HTML CSS JS** code for your website:
```js
<!DOCTYPE html>
<html>
<head>
  <title>Contact Form</title>
  <style>
    body {
      font-family: Arial;
      background-color: #fff;
    }
    form {
      max-width: 400px;
      margin: 10px auto;
      padding: 20px;
      background-color: #fff;
      border: 1px solid #ddd;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    label {
      display: block;
      margin-bottom: 10px;
    }
    input[type="text"], input[type="email"], textarea {
      width: 100%;
      height: 40px;
      margin-bottom: 20px;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    textarea {
      height: 100px;
      resize: vertical;
    }
    button[type="submit"] {
      width: 100%;
      height: 40px;
      background-color: #4CAF50;
      color: #fff;
      padding: 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button[type="submit"]:hover {
      background-color: #3e8e41;
    }
  </style>
</head>
<body>
  <form id="contactForm" onsubmit="return validateForm();" target="iframe" method="POST" action="YOUR_WEBAPP_URL">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" placeholder="name" required><br><br>

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" placeholder="email"required><br><br>

    <label for="question">Your Question:</label>
    <textarea id="question" name="question" placeholder="what about"required></textarea><br><br>

    <button type="submit">Send</button>
  </form>
<iframe name="iframe" style="position: absolute; visibility: hidden"></iframe>
<div id="successMessage" style="display: none;">
    <p>Your message has been sent successfully!</p>
  </div>

  <script>
    function validateForm() {
      const name = document.getElementById("name").value;
      const email = document.getElementById("email").value;
      const question = document.getElementById("question").value;

      // Check for empty fields
      if (name === "" || email === "" || question === "") {
        alert("Please fill in all required fields.");
        return false;
      }

      // Validate email
      const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
      if (!emailRegex.test(email)) {
        alert("Please enter a valid email address.");
        return false;
      }
    // If validation passes, submit the form and display the success message
      document.getElementById("contactForm").submit();
      document.getElementById("successMessage").style.display = "block";

      // Set a timeout to hide the success message after 15 seconds
      setTimeout(function() {
        document.getElementById("successMessage").style.display = "none";
      }, 15000);

      document.getElementById("contactForm").reset(); // Clear form fields
      return false;
    }
  </script>
</body>
</html>
```
**FINANLY FORM & SHEET RECORD**

![submit](https://github.com/user-attachments/assets/2fe2c777-a6d0-4765-986b-9c9abf6f53af)
![test mail success](https://github.com/user-attachments/assets/d4cc8a9c-bf2e-444b-9077-d38e98cea178)



