# D&H College SAT Practice Portal - Accurate Upload Version

This version is designed to make SAT question uploading more accurate while still keeping the process fast after the first setup.

## What this version adds

- Fast PDF text extraction
- OCR extraction for scanned/image PDFs
- Admin review editor for every detected question
- Split, merge, duplicate, delete, add, and renumber tools
- Manual crop builder for exact question images
- Cropped question images shown to students during practice
- English-only, Math-only, and Full Test options
- Answer key CSV upload
- Optional scoring CSV upload
- Login, admin account creation, and password reset
- Score history

## Why this is more accurate

OCR alone is never perfect for scanned SAT PDFs. It can break lines, miss punctuation, or ruin tables and diagrams. This version uses a hybrid workflow:

```text
PDF upload
→ auto extraction or OCR
→ admin review and correction
→ optional exact image crops
→ answer key matching
→ save final reviewed test
```

The admin only does this once. After the test is saved, students open it instantly.

## How to open locally

Unzip the folder. Then run:

```bash
cd dh-sat-practice-accurate-upload
python3 -m http.server 8000
```

Open:

```text
http://localhost:8000
```

This is better than double-clicking `index.html` because PDF extraction and OCR work more reliably through localhost.

## Admin setup

1. Open the website.
2. Click **Create Account**.
3. Enter a username and password.
4. Enter a security question and answer.
5. Check **Create as admin**.
6. Use this admin code:

```text
DNH-SAT-ADMIN
```

7. Click **Create Account**.

Change the admin code inside `app.js` before sharing publicly:

```js
const ADMIN_CODE = "DNH-SAT-ADMIN";
```

## Step-by-step upload guide

### Step 1: Go to Admin Upload

Log in as an admin and click **Admin Upload**.

Fill in:

```text
Test Name
Folder Date
SAT PDF
```

### Step 2: Auto Extract

Open the **1 Auto Extract** tab.

Use:

```text
Fast Text Extract
```

for PDFs where the text is selectable.

Use:

```text
Extract With OCR
```

for scanned or image-based PDFs.

OCR takes longer, but it only needs to be done once by the admin.

### Step 3: Review & Fix

Open **2 Review & Fix**.

Check every question card.

You can edit:

```text
Module
Question number
Section
Question type
Page number
Question text
```

Use:

```text
Split at Cursor
```

when two questions are combined into one. Click inside the text where the second question begins, then click the button.

Use:

```text
Merge Next
```

when one question was accidentally split into two cards.

Use:

```text
Duplicate
```

when a nearby question format is similar and you want to edit a copy.

Use:

```text
Delete
```

when OCR creates junk text or a duplicate.

Use:

```text
Add Question
```

when a question is missing.

Use:

```text
Renumber
```

to make question numbers clean within each module.

### Step 4: Crop Builder for maximum accuracy

Open **3 Crop Builder**.

Use this when OCR is messy or when the question has:

```text
tables
charts
diagrams
formulas
weird spacing
underlines
images
```

Steps:

1. Click **Load PDF Pages**.
2. Use **Previous** and **Next** to find the correct page.
3. Draw a box around the question on the page.
4. Choose the module:

```text
RW1
RW2
M1
M2
```

5. Choose the section:

```text
Reading and Writing
Math
```

6. Enter the question number.
7. Add optional text notes if needed.
8. Click **Save Crop as Question**.

If a question with the same module and number already exists, the crop updates that question. If it does not exist, it creates a new one.

### Step 5: Upload the answer key

Open **4 Answers & Save**.

Upload an answer key CSV using this exact format:

```csv
module,question,section,correct_answer,type
RW1,1,Reading and Writing,A,multiple_choice
RW1,2,Reading and Writing,C,multiple_choice
RW2,1,Reading and Writing,D,multiple_choice
M1,1,Math,12,grid_in
M1,2,Math,B,multiple_choice
```

Module names should match the question editor:

```text
RW1 = Reading and Writing Module 1
RW2 = Reading and Writing Module 2
M1 = Math Module 1
M2 = Math Module 2
```

Use `multiple_choice` for A/B/C/D questions.

Use `grid_in` for typed math answers.

### Step 6: Optional scoring table

Upload a scoring CSV only if you have the scoring table for that test.

Format:

```csv
section,raw_score,scaled_score
Reading and Writing,43,640
Reading and Writing,44,650
Math,35,650
Math,36,660
```

If you do not upload this, the website estimates scores from 200 to 800 based on percentage correct.

### Step 7: Save the test

Click:

```text
Save SAT Test
```

The test will appear in the **Test Library**.

## Student test options

Each saved SAT has three buttons:

```text
English
Math
Full Test
```

English uses only Reading and Writing modules.

Math uses only Math modules.

Full Test uses all modules.

## Best upload method by PDF type

### Clean selectable PDF

```text
Upload PDF
→ Fast Text Extract
→ Review & Fix
→ Upload answer key
→ Save
```

### Scanned PDF

```text
Upload PDF
→ Extract With OCR
→ Review & Fix
→ Crop messy questions
→ Upload answer key
→ Save
```

### Very messy scanned PDF

```text
Upload PDF
→ Use Crop Builder for every question
→ Upload answer key
→ Save
```

This takes longer for the admin but gives students the most accurate experience.

## Important storage note

This is still a static browser-based website. It stores uploaded PDFs, extracted questions, crops, accounts, and scores in the browser using localStorage and IndexedDB.

That means:

```text
Your data stays in the same browser.
Clearing browser site data can delete tests and scores.
Different computers do not automatically share uploaded tests.
```

For a real shared school portal, use a backend like Supabase or Firebase.

## Files included

```text
index.html
styles.css
app.js
README.md
sample-answer-key.csv
sample-scoring-table.csv
```

## Troubleshooting

### OCR does not work

Make sure you are connected to the internet because the OCR library loads from a CDN.

### PDF pages do not load

Run the website through localhost:

```bash
python3 -m http.server 8000
```

### Questions are split badly

Use **Review & Fix** with Split, Merge, Delete, Add, and Renumber.

### A question has a chart or table

Use **Crop Builder** and save the exact question image.

### Scores look wrong

Upload the correct scoring table for that SAT.
