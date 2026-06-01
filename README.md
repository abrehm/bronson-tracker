# Bronson Tracker

Standalone HTML workout tracker. Phone-first. Saves session logs as markdown files you push to Dropbox, where Cowork on Mac reads them.

## What this is

A single-file webapp. No build step, no dependencies, no backend. Open it in any modern browser. Data lives in your browser's localStorage; session logs are downloaded as markdown files when you save.

## Privacy note before you deploy

The HTML file itself contains no personal data — it's just program templates and UI. Your actual workout data is stored in:
- Your phone's browser localStorage (private to your device)
- Markdown files you download and save to your Dropbox

Hosting this file publicly does not expose your data. The repo can be public. But if you'd rather keep it private, see "Hosting alternatives" below.

## Deployment — GitHub Pages

Most beginner-friendly path. Free, takes ~5 minutes.

1. **Create a new repository** on GitHub. Name it whatever you want (e.g. `bronson-tracker`). Set it to **Public** (Pages on free GitHub requires public; paid plans support private).
2. **Upload `index.html`** to the repository root. Use the web upload — drag-drop on github.com.
3. **Enable Pages**: Settings → Pages → Source: "Deploy from a branch" → Branch: `main` (or `master`), folder: `/ (root)` → Save.
4. **Wait ~1 minute.** GitHub builds and deploys. The Settings page will show your URL: `https://YOUR-USERNAME.github.io/bronson-tracker/`
5. **Visit the URL** on your iPhone. The tracker should load.

## Add to iPhone home screen

In Safari on iPhone:
1. Open your tracker URL
2. Tap the Share button (square with up-arrow)
3. Scroll down, tap **Add to Home Screen**
4. Name it "Bronson" (or whatever), tap Add

Now it's an icon on your home screen. Tap it, opens full-screen, looks and behaves like a native app.

## First-use checklist

1. Tap the Bronson icon on your home screen
2. Go to **SETUP** tab → set your **Program Start Date** → save
3. Stay in SETUP → scroll to about section, read it
4. Go back to **TODAY** → fill in your **Baseline Measurements** (or use `TBD` markers for things you haven't measured yet)
5. Test the save flow with a fake session before your real day-one:
   - Wait until your start date arrives (or temporarily set start date to today)
   - Log a fake session with random reps
   - Tap **SAVE SESSION**
   - iOS should show a download notification, then a "Save to Files" option
   - Save to **Dropbox → OurStuff → Andrew-Workout → sessions**
6. On Mac: confirm the .md file appeared in your Dropbox folder
7. Delete the fake session file from Dropbox if you don't want it polluting your Cowork analysis

## Daily workflow once running

In the basement:
1. Tap the Bronson icon
2. Log your reps as you train (taps work without internet — page is cached)
3. After your last set, tap **SAVE SESSION**
4. Pick **Save to Files → Dropbox → Andrew-Workout → sessions** (iOS remembers the path after first time)

On Mac, weekly or whenever:
- Open your Cowork project → it reads new session files from the Dropbox folder

## Backup discipline

The tracker has an **EXPORT FULL BACKUP** button in SETUP. Tap it weekly. It downloads a `bronson-backup-YYYY-MM-DD.json` file with your entire local state. Save to Dropbox. If you ever lose your phone, replace your browser, or wipe localStorage by accident, you can use **IMPORT BACKUP** on a fresh device to restore everything.

## Hosting alternatives

If you'd rather not use a public repo:

- **Cloudflare Pages**: free, supports private repos. Connect a private GitHub repo, deploys automatically.
- **Netlify Drop**: drag the HTML file in, get a URL. Free. URL is less polished than custom GitHub Pages.
- **Local file in Dropbox**: save `index.html` into your Dropbox, on iPhone open via Dropbox app → "Open in Safari." Works but launching is more taps.

## Updating the tracker

When the program changes (e.g. moving to Phase II exercises, or adding features), edit `index.html` and push to GitHub. Within ~1 minute the deployed site updates. On iPhone, reload the page once to pull the new version.

Your localStorage data persists across version updates as long as the storage schema doesn't change.

## When things go wrong

- **Page won't load on phone**: check the GitHub Pages URL is correct, check Pages is enabled in Settings, give it ~5 min after first deploy
- **localStorage seems empty after browser update**: import your last backup JSON
- **iOS "Save to Files" doesn't show Dropbox**: open the Dropbox app on your phone, sign in, then try again
- **Sessions exist in browser but no .md files in Dropbox**: the download step is separate from the save step — every session save triggers a download but you have to manually pick Dropbox each time (until iOS remembers the path)

## Related files in the Dropbox folder

- `INSTRUCTIONS.md` — coaches Cowork on how to read this data
- `program.md` — the full 12-week program spec
- `baseline.md` — your baseline measurements (separate from the tracker's local copy)
- `sessions/` — where your .md files go after each workout
- `analysis/` — where Cowork writes its weekly review reports

## Notes for Everypoint experiment

This file demonstrates a deployable pattern: client-side webapp + plain-file outputs + cloud sync + desktop AI agent for analysis. Total stack cost: $0. Total infrastructure: a GitHub repo and a Dropbox folder. The pattern generalizes to any "field operative captures structured data → office analyst reviews" workflow.

Key properties worth noting:
- No backend to maintain
- No API keys or auth flows
- Data never traverses any server you don't already use
- Works offline after first load
- Total code in one file, ~50KB
- Editable by hand if needed
