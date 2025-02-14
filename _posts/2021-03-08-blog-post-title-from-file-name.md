## Stopping Bill Murray Syndrome

For a long time I had the feeling that each week was a new start to my sales journey. I struggled to chain days and weeks together, which made my follow-up's inconsistent. I was like the family cat that disappears only to come back 6 months later, without any acknowledgement that I am weird and mysterious.

The main reason for my Groundhog Day sales journey was that we're a bootstrapped company and our founder is as tough as early Amazon Jeff B. in controlling costs. Combined with the horrible design of most CRM's and our record keeping was spread out across an ever expanding number of Google Sheets.

This week, I decided to change that. My goal is to have a clear view of who I will follow-up with next week from this week. 

---

### Exporting from Google

The Gmail design is always changing slightly, which makes most tutorials on the internet obselete. 

For example, there were a lot of pages suggesting that Gmail had a "download" feature when you clicked on the 3 dots button. That is no more. 

To export from Gmail, we have 2 options. The first is form Takeout, but that only allows us to export ALL our emails.

I only want to export SENT emails from a certain day or 5 day business week. 

is:sent from:(email address) after:2025/month.number/day.number before:2025/month.number/day.number

is the way to go.

Once we have a list of emails, we can select Forward as Attachment in the 3 dots button.

![Google Apps Scrips screenshot](https://github.com/cerulean-orca/blog-images/blob/main/Forward%20as%20attachment%20screenshot.png)"

We get compressed meta data of our messages as an email - which we could then upload to Google Drive.

#### Google Apps Script

![Google Apps Scrips screenshot](https://raw.githubusercontent.com/cerulean-orca/blog-images/refs/heads/main/EMLF-script-snippet.png)"

Once our files are in Google Drive, all we have to do to get them in our Google Sheets is run the script below:

#### EMLF Google Script

```function processEMLFiles() {
  try {
    // Get the active spreadsheet
    const ss = SpreadsheetApp.getActiveSpreadsheet();
    const sheet = ss.getSheetByName('Email Data') || ss.insertSheet('Email Data');
    
    // Set up headers
    const headers = ['Filename', 'From', 'To', 'Date Sent'];
    sheet.getRange(1, 1, 1, headers.length).setValues([headers]);
    
    // Get all files in the user's Drive with the correct MIME type for EML files
    const files = DriveApp.getFilesByType('message/rfc822');
    let row = 2; // Start after headers
    let processedFiles = 0;
    
    // Process each EML file
    while (files.hasNext()) {
      const file = files.next();
      Logger.log('Processing file: ' + file.getName());
      
      try {
        const content = file.getBlob().getDataAsString();
        
        // Extract and clean metadata
        const filename = file.getName();
        const from = cleanEmailField(extractHeader(content, 'From'));
        const to = cleanEmailField(extractHeader(content, 'To'));
        const date = extractHeader(content, 'Date');
        
        // Write to spreadsheet
        sheet.getRange(row, 1, 1, 4).setValues([[
          filename,
          from,
          to,
          date
        ]]);
        
        row++;
        processedFiles++;
      } catch (fileError) {
        Logger.log('Error processing file ' + file.getName() + ': ' + fileError.toString());
        continue;
      }
    }
    
    Logger.log('Processing complete! Processed ' + processedFiles + ' files');
    
    // Auto-resize columns to fit content
    sheet.autoResizeColumns(1, headers.length);
    
  } catch (error) {
    Logger.log('Error: ' + error.toString());
    throw error;
  }
}

function extractHeader(content, headerName) {
  const regex = new RegExp(`^${headerName}: (.*)$`, 'mi');
  const match = content.match(regex);
  return match ? match[1].trim() : '';
}

function cleanEmailField(emailString) {
  if (!emailString) return '';
  
  // Handle both formats: "Name <email>" and "<email>"
  let cleanedString = emailString;
  
  // Strip any quotes at the beginning and end
  cleanedString = cleanedString.replace(/^"|"$/g, '');
  
  // Extract just the email address if it's in angle brackets
  const emailMatch = cleanedString.match(/<([^>]+)>/);
  if (emailMatch) {
    return emailMatch[1]; // Return just the email address without brackets
  }
  
  return cleanedString;
}
```

Great!

We can now run a weekly transfer of our sent emails and make them static in Google Sheets.

This is maybe a slight leap forward, but paves the way for us to stop slipping deals. 

