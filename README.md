# CUC Secondary School — Subject Selection Form

An interactive subject selection form for Form 3 students at Caribbean Union College Secondary School, Maracas Royal Road, St. Joseph, Trinidad and Tobago.

Students make their subject choices through a guided, clickable interface. When they submit, the form opens the school's Google Form pre-filled with their selections. All responses are collected in the existing Google Sheet.

The form includes a hidden admin reference panel — accessible to the principal and website maintainer — with instructions for all common tasks and a tool for verifying Google Form connections.

---

## What's in this repository

| File | Purpose |
|---|---|
| `cuc_subject_selection.html` | The complete form — open this in any browser |
| `cuc-logo.png` | The school logo (place in the same folder as the HTML) |
| `README.md` | This document |

---

## For the website maintainer — hosting

1. Download `cuc_subject_selection.html` and `cuc-logo.png` from this repository
2. Place both files in the same folder on the web server
3. Link to `cuc_subject_selection.html` from the school website

To download a file from GitHub: click the filename, then click the **Download raw file** button (arrow icon, top right of the file view).

No server-side setup, database, or installation required. The form works immediately after hosting.

---

## The admin reference panel

A hidden panel is built into the form with instructions for the principal and the website maintainer. It contains a Google Form URL verifier tool.

**To open it:**
- **Desktop:** Press `Ctrl + Shift + A` anywhere on the form page
- **Phone / tablet:** Tap the school name in the blue header five times quickly (within three seconds)

Enter the admin password when prompted.

The panel has four tabs: **Google Form**, **For the Principal**, **For the Maintainer**, and **About**.

---

## Managing the form — options

### Closing the form immediately (principal does this himself)

The fastest option — no technical help needed.

1. Open the Google Form in Edit mode
2. Click the **Responses** tab
3. Toggle **Accepting responses** off

Students who try to submit will see Google's own "no longer accepting responses" message. To reopen, toggle it back on.

---

### Closing the form on the school website (website maintainer)

This replaces the interactive form with a custom closed message on the school website. It requires a file update.

Find this line near the top of the HTML file:

```js
const FORM_OPEN = true;
```

Change `true` to `false`. Save the file and re-upload it to the server. Students will then see the closed message instead of the form.

To customise the closed message, find and edit:

```js
const CLOSED_MESSAGE = "Subject selection for 2026–2027 is now closed...";
```

To reopen the form, change `FORM_OPEN` back to `true` and re-upload.

---

### Connecting a new Google Form each year (principal + developer + maintainer)

Each year, when the principal creates a new Google Form, the HTML file needs to be updated with the new form's connection details. The process:

1. **Principal** creates the new Google Form (or copies the previous one)
2. **Principal** generates a pre-filled link (instructions in the admin panel → Google Form tab) and verifies it using the built-in tool
3. **Principal** sends the verified pre-filled link to the website maintainer
4. **Website maintainer** updates the `GOOGLE_FORM_URL` and `ENTRY_IDS` in the HTML file and saves the updated file
5. **Website maintainer** uploads the updated file to the server

---

## For the developer — updating the source each year

Open the HTML file in a plain text editor (Notepad on Windows, TextEdit on Mac in plain text mode — never Word or Google Docs). All editable config is near the top of the file.

### Academic year

```js
const ACADEMIC_YEAR = "2026–2027";
```

Updates the header, footer, and pledge text automatically.

---

### Form open / closed

```js
const FORM_OPEN = true;
```

Change to `false` to show the closed message. Change back to `true` to reopen.

---

### Closed message

```js
const CLOSED_MESSAGE = "Subject selection for 2026–2027 is now closed. Please see your form teacher.";
```

---

### Google Form URL and entry IDs

```js
const GOOGLE_FORM_URL = "https://docs.google.com/forms/d/e/YOUR_FORM_ID/viewform";

const ENTRY_IDS = {
  lastName:       "entry.771988839",
  middleName:     "entry.474596500",
  firstName:      "entry.2039237738",
  gender:         "entry.1645034372",
  formClass:      "entry.2035351854",
  studentEmail:   "entry.994325373",
  studentPhone:   "entry.1166974658",
  parentPhone:    "entry.223910069",
  careerInterest: "entry.1458099417",
  group4_1st:     "entry.49639083",
  group4_2nd:     "entry.854191362",
  group5_1st:     "entry.2057983962",
  group5_2nd:     "entry.1380278852",
  group6_1st:     "entry.1035308001",
  group6_2nd:     "entry.754522490",
  group7_1st:     "entry.407558374",
  group7_2nd:     "entry.59725307",
  group8_1st:     "entry.1538596846",
  group8_2nd:     "entry.1763167778",
  additional:     "entry.622064300",
  parentSig:      "entry.1477906076",
  studentSig:     "entry.653844226",
};
```

Replace these with values from the principal's new pre-filled link. The admin panel's verifier tool confirms the mapping before you update.

---

### Form classes

```js
const FORM_CLASSES = [
  "3A - Amethyst",
  "3B - Beryl",
  "3C - Chrysolite",
];
```

Add, rename, or remove entries. Keep quotation marks and commas exactly as shown.

---

### Subject groups

Find the `GROUPS` array. Each group looks like this:

```js
{ num: 4, subjects: [
  { name: "Biology" },
  { name: "Integrated Science" },
  { name: "Food and Nutrition*", tech: true },
]},
```

- Edit the name in quotes to rename a subject
- Add a new line to add a subject
- Delete a line to remove a subject
- Technical subjects must have `tech: true` — displays them in italics with an asterisk
- If you rename a subject that appears in `SUBJECT_FAMILIES`, update it there too

---

### Subject families

Subjects that appear in more than one group are linked so a student cannot choose the same subject as a first choice in two groups.

```js
const SUBJECT_FAMILIES = {
  "Spanish (Group 1)": "spanish",
  "Spanish (Group 2)": "spanish",
  ...
};
```

If subject names change, update here to match exactly. If a new subject appears in two groups, add both names with a shared lowercase family label.

---

### Additional subjects

```js
const ADDITIONAL_SUBJECTS = [
  { name: "Religious Education", evening: false },
  { name: "French (evening school)", evening: true },
];
```

`evening: true` adds a note about potential additional cost beneath the subject name.

---

### School logo

Place the logo file in the same folder as the HTML file and update:

```js
const LOGO_FILE = "cuc-logo.png";
```

Leave as `""` to show the text fallback.

---

### Subject descriptions (tooltips)

Each subject has a short hover description in the `SUBJECT_INFO` object. Find the subject by name and edit. Keep under 30 words. Add new entries to match any new subjects added to the groups.

---

## How the form works — summary for staff

1. Student opens the form on the school website
2. Student fills in personal details
3. Student clicks to select a 1st and 2nd choice from each of the 5 groups
4. The form prevents the same subject being chosen as a 1st choice in two groups
5. Student ticks any additional CSEC subjects
6. Student and parent type their signatures
7. Student clicks **Submit to school**
8. The school's Google Form opens pre-filled in a new tab
9. Student reviews and clicks Submit
10. Response recorded in Google Sheet as normal

---

## Technical notes

- Single HTML file — no server-side code, database, or installation required
- Works in all modern browsers on desktop, phone, and tablet
- Internet connection required (Google Fonts and Google Form submission)
- No student data stored in this file or on GitHub
- GitHub repository is public — contains no sensitive credentials or student data

---

## Maintained by

Andrew Legall  
CUC Secondary School  
*Last updated: 2026*
