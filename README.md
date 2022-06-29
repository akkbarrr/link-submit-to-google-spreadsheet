# Link submit button in form website to google spreadsheet

in this repo you will get to know that how you can send and store HTML/PHP form data into Google/Excel sheet.

## Usage

- Add html script tag into your html file in the body tag and put the script tag inside the body tag.
```javascript
const scriptURL =
'LINK-HERE'
const form = document.forms['google-sheet']

form.addEventListener('submit', e => {
     e.preventDefault()
     fetch(scriptURL, { method: 'POST', body: new FormData(form)})
        .then(response => alert("Terimakasih sudah submitting ke kami..!"))
        .catch(error => console.error('Error!', error.message))
})
```
- in the label tag, add __for="example"__ attribute.
- in the tag input, add __<input type="text" id="fname" name="example" placeholder="example name" required>__
![ss-amcrit2](https://github.com/akkbarrr/link-submit-to-google-spreadsheet/blob/main/img/ss-amcrit2.png?raw=true)
- Create a new spreadsheet and add the name for your data ___don't forget to 
match the name of the data in the spreadsheet with the same name in the input tag___.
![ss-amcrit](https://github.com/akkbarrr/link-submit-to-google-spreadsheet/blob/main/img/ss-amcrit.png?raw=true) 
- Go to __Extension__ and click __App Script__.
- Change the name of the app script file into the same name as gspreadsheet file
- Add this script to scrape and link your form website to gspreadsheet __remember, if you want to change your Sheet1 name, you must change the name in the sheetName variable too__
```javascript
var sheetName = 'Sheet1'
    var scriptProp = PropertiesService.getScriptProperties()

    function initialSetup () {
      var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
      scriptProp.setProperty('key', activeSpreadsheet.getId())
    }

    function doPost (e) {
      var lock = LockService.getScriptLock()
      lock.tryLock(10000)

      try {
      var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
      var sheet = doc.getSheetByName(sheetName)

      var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValue()[0]
      var nextRow = sheet.getLastRow() + 1

      var newRow = headers.map(function(header) {
        return header === 'timestamp' ? new Date() : e.parameter[header]
      })

      sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

      return ContentService
        .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow}))
        .setMimeType(ContentService.MimeType.JSON)
      }

      catch (e) {
      return ContentService
        .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e}))
      }

      finally {
        lock.releaseLock()
      }
    }
```
- Create a new deployment and create new web application and change the access to anyone and copy the URL and put the URL in the html script, try to submit and your form data will appears in gspreadsheet.


## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
© Akbar IDN, SMK IDN Boarding School