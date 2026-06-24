# How to make Karnataka Trip Planner a PWA

## Step 1 — Copy these files into your project folder

Copy ALL of the following into the SAME folder as your index.html:
- manifest.json
- sw.js
- icons/ (entire folder with all 8 PNG files)

Your folder should look like:
```
karnataka-trip-planner/
├── index.html
├── manifest.json        ← new
├── sw.js                ← new
├── icons/               ← new folder
│   ├── icon-72.png
│   ├── icon-96.png
│   ├── icon-128.png
│   ├── icon-144.png
│   ├── icon-152.png
│   ├── icon-192.png
│   ├── icon-384.png
│   └── icon-512.png
├── README.md
├── firebase-setup.md
└── .gitignore
```

## Step 2 — Add to index.html HEAD section

Open index.html in VS Code.
Find the line:  <meta name="theme-color" content="#E8692A">

PASTE the contents of ADD-TO-HEAD.txt AFTER that line.

## Step 3 — Add to index.html BODY section

Find the closing </body> tag at the very bottom of index.html.

PASTE the contents of ADD-TO-BODY.txt JUST BEFORE </body>

## Step 4 — Save and push to Firebase Hosting

In VS Code terminal:
  git add .
  git commit -m "feat: added PWA support - app installable on phone"
  git push
  firebase deploy --only hosting

## Step 5 — Install on your phone

Open roamly-tripplanner.web.app on your phone browser.

Android (Chrome):
  - Tap the 3-dot menu → "Add to Home screen" → Add
  - OR tap the "Install Roamly as an app" banner that appears

iPhone (Safari):
  - Tap the share icon (box with arrow)
  - Scroll down → tap "Add to Home Screen"
  - Tap "Add"

The app now appears on your home screen with the orange Roamly icon.
Opening it from the home screen = fullscreen, no browser bars, looks exactly like a native app.
