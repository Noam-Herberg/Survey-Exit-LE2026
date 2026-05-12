# Campaigner survey

Open `index.html` in a browser to use the survey.

## Google Sheet backend

The page is ready to post responses to a Google Apps Script web app. After creating and deploying the Apps Script, paste its web app URL into this line in `index.html`:

```js
const GOOGLE_SHEET_ENDPOINT = "";
```

Until that URL is added, the form downloads each submitted response as a CSV file.

Example Apps Script:

```js
const SHEET_NAME = "Responses";

function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
  const data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.submittedAt,
    data.authority,
    data.leafletKinds,
    data.phonebankingHours,
    data.canvassingHours,
    data.leafletDeliveryHours,
    data.resultsSurprising
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

Suggested sheet columns:

```text
submittedAt, authority, leafletKinds, phonebankingHours, canvassingHours, leafletDeliveryHours, resultsSurprising
```
