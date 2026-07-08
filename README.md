# DHFTHLoaf

# Setup

### Create Google Form

### Add Email field and a First Name Field

### Open the Google Form's linked Google Apps Form
- Click the 3 dots in the upper right corner
- Click App Scripts

### Add to Google App Script Library
- Open your Google App Script
- Click plus button next to Libraries
- Lookup the Script Id 
Script Id:
```
12rvfsDfCwkpfmiPfh6fOoh_my2xU2E0pQoHvzayANg1XZLzwtSaoZ_Lh
```
- Name the identifier to `Loaf`

### Boilerplate code for FTH club

```
let club = Loaf.createClubModel(
    "<club_id>", 
    "<Club Name>",
    "<FTH_User_Name>", "<FTH_Password>",
    "America/Los_Angeles", //<- IANA Timezone (PST/PDT)
    []
  )
```

```
async function formSubmitted(e) {  
  let guest = Loaf.createGuestRSVPModelFromEvent(e)
 
  let mailObject = await Loaf.LoafServiceHandleRSVPSubmission(guest, club, true)
  MailApp.sendEmail(mailObject)
}

async function sendReminderEmails() {
  let form = FormApp.getActiveForm()
  let mailObjects = await Loaf.LoafServiceRemindEmailForMembersAndGuests(form, club, "", true)

  mailObjects?.forEach(elm => {
    MailApp.sendEmail(elm)
  })
}
```

### Boilerplate code for Non-Charted club
```
let club = Loaf.createSeedModel(
  "<Club_Name>",             // e.g. "ABC"
  "<day_name>",              // e.g. "wednesday"
  "<time>",                  // e.g. "6:00 pm"
  "<iana_timezone>",         // e.g. "America/Los_Angeles"
  "<meeting_location>",      // e.g. "www.zoom.com/12356789" or "123 Awesome St"
  "<primary_contact_email>"  // e.g. "your_email@gmail.com"
)
```

```
async function formSubmitted(e) {
  let guest = Loaf.createGuestRSVPModelFromEvent(e) 

  let mailObject = await Loaf.SeedServiceHandleRSVPSubmission(guest, club)
  MailApp.sendEmail(mailObject)
}

async function sendReminderEmails() {
  let form = FormApp.getActiveForm()
  let mailObjects = await Loaf.SeedServiceRemindEmailForGuestSeeds(form, club)

  mailObjects?.forEach(elm => {
    MailApp.sendEmail(elm)
  })
}
```

### Set Up Triggers 

1. Form submission trigger
Triggers > + Add Trigger 

Select `formSubmitted` function to run

Change event type to `On form submit`

Click Save

2. Email Reminder Trigger
Triggers > + Add Trigger 

Select `sendReminderEmails` function to run

Change event source to `Time-driven`

Change type of time-based trigger to `Week timer`

Change day of week to the day of your meeting

Change time of day to the hour you want the reminder to go out. (**note: The trigger will go out based on the value of the hour you choose.  Be aware that it is in respect to the timezone shown at the bottom.  For example if it says GMT-8 then thats probably PST.)

Click Save


### Linked Sheet
Go to your Form and click on the "Responses" Tab.  Click on "View in Sheets".  If it hasn't been created it will ask you to create.  

When `LoafServiceRemindEmailForMembersAndGuests` is called, the email will contain a link to that spreadsheet, but only to members.  The guests wouldn't get it.

When `SeedServiceRemindEmailForGuestSeeds` is called, the email will contain the link to that spreadshet, but only to the primary contact email.  Everyonesle wouldn't get it.

### Debug run
At the top of the editor, click the dropdown and select, `sendReminderEmails`.  If that option isn't there, you may need to save first.

Then click run.  If this is the first time, you will need to grant permissions. A window will open up.  Click on "unsafe" or "Advanced" link.  It will show you what permissions it needs to run.  Select all of them then continue.  

If everything went as planned, your club should have gotten a nice email reminder about the upcoming meeting!






