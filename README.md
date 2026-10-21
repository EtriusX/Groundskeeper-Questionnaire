# GROUNDSKEEPER — NEW STEAM GALLERY STUDY

This project is written from scratch. It contains all 30 user-provided screenshots (converted to optimized WebP), a four-comparison randomized king-of-the-hill tournament, ordered top-five supplementary screenshot selection, seven multiple-choice questions plus optional comment, and a name/pseudonym field.

**No email delivery, no old centerpiece-picking stage, and no manual screenshot submission.** The site writes to Google Sheets through the server. Successful submission appears ONLY if Google Sheets confirms receipt.

## 1 — Preview on Windows

Install Node.js 20+ from https://nodejs.org, extract this ZIP, and double-click `START-LOCAL-WINDOWS.bat`. The page opens at http://localhost:3000.

**Important:** A local preview cannot complete the final submission before you configure Google Sheets. This is intentional: it must not falsely claim a response was saved.

## 2 — Create the Google Sheet

1. Open Google Sheets at https://sheets.google.com and create a blank spreadsheet named `Groundskeeper Study Responses`.
2. Choose **Extensions > Apps Script**.
3. Replace the contents of `Code.gs` with the entire code in the supplied `GoogleAppsScript.gs` file, then save.
4. In Apps Script **Project Settings > Script Properties**, add property `SHEETS_SECRET` with a long randomly created secret (32+ characters). Keep it private. You will also add the same secret to Railway.
5. From the Apps Script editor, select `initialize` in the function chooser and click **Run** once. Approve Google's permission prompts. This creates three tabs: `Participants`, `Rankings`, and `Supplementary Scores`.
6. Select **Deploy > New deployment > Select type > Web app**. Choose **Execute as: Me** and **Who has access: Anyone**. Deploy and authorize if prompted.
7. Copy the full generated **Web app URL** ending in `/exec`. This is the URL for `SHEETS_WEBHOOK_URL`, NOT the regular Google Sheets page URL.
8. When you change Apps Script code later, create a **new deployment version** (Deploy > Manage deployments > Edit > Version: New version > Deploy) or the old deployed code remains active.

**Security note:** The Apps Script URL accepts public HTTP requests, but rejects submissions without the shared `SHEETS_SECRET` checked server-side. The secret is never added to browser code. Since the form is a public voluntary experiment, it does not have CAPTCHA or strong identity verification; someone could submit more than once under different names.

## 3 — Deploy new site on Railway

1. Create a **new empty GitHub repository**; this avoids all duplicate-folder and stale-source problems from the previous site.
2. In File Explorer open the extracted folder containing `server.js`, `package.json`, `public/`, `GoogleAppsScript.gs`, and this README.
3. In the *GitHub repository root* choose **Add file > Upload files**. Drag in **the contents of that folder**, not the ZIP and not an extra enclosing folder. Preserve `public/index.html`, `public/app.js`, `public/style.css`, and `public/assets/...`.
4. Create a **new Railway project** using `Deploy from GitHub Repo`, select the new repo, and generate a domain under the service's **Settings > Networking**. Railway detects Node.js and runs the `npm start` script; the server honors Railway's `PORT` environment variable.
5. In Railway service **Variables**, add:
   * `SHEETS_WEBHOOK_URL` = URL ending `/exec` from Apps Script deployment.
   * `SHEETS_SECRET` = same secret set in Apps Script Project Settings.
6. Let Railway redeploy. Visit `https://YOUR-DOMAIN/health` to verify `homepageExists: true` and `sheetsConfigured: true`.
7. In your browser open the new domain in a **fresh tab**. Complete all four comparisons, rank five images, answer all questions, add a pseudonym, and click **Complete test & save results**. Only a successful save produces the thank-you screen.
8. Check all three tabs of the Google Sheet. `Participants` gets one row with the name, winner, matchups, ranked screenshots, and questionnaire. `Rankings` gets five rows per participant with point values 5–1. `Supplementary Scores` automatically sums each screenshot's points.

**Do not change the scores manually:** They update from the Rankings tab using formulas. Sort or filter the `Supplementary Scores` tab by **Total Points** to see the most popular screenshots for the proposed Ultra Steam Page.

## 4 — Research and data cautions

* Five collections A–E are shuffled independently per participant. Four winner-stays-on comparisons follow that order. The exact order and all left/right matchups are saved.
* The winner of this format is not a complete ranking of all five collections; it is sensitive to presentation order. Collect enough participants and report this limitation.
* Ranking points are a defined preference score (5/4/3/2/1), not an objective picture-quality measure. Combining five highest-ranked supplements with the winning hero yields a *proposed* ultimate gallery, not proven optimal without retesting.
* Google Sheet data is stored in the researcher's Google account; choose a retention policy and disclose that participation data (including entered name/pseudonym) is saved.
* Keep credentials (both secrets and any other tokens) out of GitHub and out of screenshots.
* There is basic submission validation and duplicate session protection in Google Sheets. This is not an anti-bot or fraud-resistant research survey.

## Files

`public/` website front end and 30 images; `server.js` Node.js server + validated Google Sheets forwarding; `GoogleAppsScript.gs` code installed in Google Sheet; `START-LOCAL-WINDOWS.bat` local Windows preview; `package.json` and README.
