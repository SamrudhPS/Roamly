# Firebase Setup Guide

This guide walks you through setting up Firebase Authentication and Firestore for the Karnataka Trip Planner. Takes ~10 minutes.

**Before you start:** the app works in DEMO MODE without Firebase (using localStorage). Set up Firebase only when you want real user accounts that sync across devices.

---

## Step 1 — Create a Firebase Project

1. Go to https://console.firebase.google.com
2. Click **Add project**
3. Project name: `karnataka-trip-planner` (or any name)
4. Disable Google Analytics (not needed for college projects)
5. Click **Create project**, wait 30 seconds, then **Continue**

---

## Step 2 — Add a Web App

1. On your Firebase project home, click the **`</>`** (web) icon
2. Nickname: `Karnataka Trip Planner Web`
3. **Do NOT** check "Set up Firebase Hosting" (optional, do later)
4. Click **Register app**
5. Copy the `firebaseConfig` object that appears. It looks like this:

```js
const firebaseConfig = {
  apiKey: "AIzaSyB...",
  authDomain: "karnataka-trip-planner.firebaseapp.com",
  projectId: "karnataka-trip-planner",
  storageBucket: "karnataka-trip-planner.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

6. Click **Continue to console**

---

## Step 3 — Enable Email/Password Login

1. In Firebase Console left sidebar, click **Build → Authentication**
2. Click **Get started**
3. Click the **Sign-in method** tab
4. Click **Email/Password**, enable the first toggle, click **Save**

### (Optional) Enable Google Login

5. Click **Add new provider → Google**
6. Toggle **Enable** on
7. Set "Project support email" to your email
8. Click **Save**

---

## Step 4 — Set up Firestore Database

1. In left sidebar, click **Build → Firestore Database**
2. Click **Create database**
3. **Location**: choose `asia-south1 (Mumbai)` for best India performance
4. **Start in test mode** (lets anyone read/write for 30 days — fine for college)
5. Click **Create**

Test mode is fine for development. For production, see "Security Rules" below.

---

## Step 5 — Paste Config into Your App

Open `index.html` in VS Code. Find this section near the top of the `<script type="module">` block:

```js
const FIREBASE_CONFIG = {
  apiKey: "YOUR_API_KEY_HERE",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Replace it with the config from Step 2. Save the file.

---

## Step 6 — Test It

1. Open `index.html` with Live Server in VS Code
2. You should see a **green "FIREBASE" badge** in the top right (instead of orange "DEMO MODE")
3. Create a new account using Sign up
4. Refresh the page — you should still be logged in
5. Open Firebase Console → Authentication → Users — your new user appears
6. Open Firebase Console → Firestore Database → Data — a `users` collection appears with your data

If you see the green Firebase badge, **you're done**. The app now uses real authentication and cloud storage.

---

## Step 7 — Add Authorized Domain (for hosting later)

When you deploy to GitHub Pages or any URL:

1. Firebase Console → Authentication → Settings → Authorized domains
2. Click **Add domain**
3. Add your domain (e.g., `yourname.github.io`)
4. Save

Localhost is authorized by default.

---

## Security Rules (when going to production)

In Firestore → Rules tab, replace with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

This ensures users can only read/write their own data. Test mode is fine for college, but switch to this before deploying publicly.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Badge still says "DEMO MODE" | You forgot to save `index.html` after pasting config. Save and refresh. |
| `Firebase: Error (auth/invalid-api-key)` | Wrong API key. Copy carefully from Project Settings → General. |
| `Missing or insufficient permissions` | Firestore isn't in test mode or rules are too strict. Re-check Step 4. |
| Google login popup blocked | Allow popups in browser settings, or use email login. |
| Data not syncing across devices | Make sure you're logged in with the same email on both devices. |

---

## Free Tier Limits

Firebase free tier (Spark plan):
- **Authentication**: unlimited users
- **Firestore**: 50K reads, 20K writes, 1GB storage per day
- **More than enough** for any college project or demo

You won't get billed unless you upgrade manually.
