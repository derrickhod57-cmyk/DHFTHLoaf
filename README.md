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

Change time of day to the hour you want the reminder to go out.

Click Save

**note: Time of day is dependent on the timezone.  To check the timezome of your script, go to. Project Settings and change it.

### Linked Sheet







