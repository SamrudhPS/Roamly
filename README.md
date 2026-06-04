# Karnataka Trip Planner

A mobile-friendly travel planning app for Karnataka, India. Features include 25 curated destinations, intelligent itinerary generation based on traveler type, budget planning, hotel suggestions, and user accounts with cloud sync.

## Features

- **User authentication** — email/password and Google sign-in via Firebase
- **Cloud storage** — saved places, recent views, and trip preferences sync across devices
- **Smart itinerary engine** — adapts to traveler type (couple, family, elders, kids, youth, solo), travel mode (car, bus, train, bike), pace, and interests
- **Budget allocation** — auto-splits across transport, accommodation, food, entry fees
- **Trip simulation** — day-by-day plan with travel fatigue scoring and weather outlook
- **Customization** — add, remove, or swap stops from your itinerary
- **Sponsored hotel recommendations** — curated stays at each destination
- **Local breakfast & food spot suggestions** — with prices and descriptions

## Tech Stack

- **Frontend**: HTML5, CSS3, Vanilla JavaScript (no build step needed)
- **Backend**: Firebase Authentication
- **Database**: Firestore (NoSQL cloud database)
- **Fallback**: localStorage (when Firebase isn't configured)

The entire app is a single `index.html` file. No npm, no webpack, no React. Just open and run.

## Quick Start

### Option 1: Demo Mode (No Setup Required)

1. Open `index.html` in any modern browser
2. Or use VS Code's Live Server extension (right-click `index.html` → Open with Live Server)
3. Sign up with any email/password — accounts are saved in your browser's localStorage

You'll see an orange "DEMO MODE" badge in the top right.

### Option 2: Production Mode (Firebase Enabled)

1. Follow the step-by-step guide in `firebase-setup.md`
2. Paste your Firebase config into `index.html`
3. Reload the page — the badge turns green ("FIREBASE")
4. User accounts are now real and sync across devices

## Test on Your Phone

Your phone and laptop must be on the same WiFi network.

1. Find your laptop's local IP:
   - Windows: `ipconfig` (look for IPv4 Address, e.g., `192.168.1.5`)
   - Mac/Linux: `ifconfig | grep inet`
2. Start Live Server in VS Code (it runs on port 5500)
3. On your phone browser, go to: `http://YOUR_IP:5500` (e.g., `http://192.168.1.5:5500`)
4. The app loads on your phone

Any save in VS Code auto-refreshes on your phone.

## Push to GitHub

```bash
# First-time Git setup (skip if done)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Create a repo on GitHub.com first (any name, public, no README)
# Then in this folder:
git init
git add .
git commit -m "initial commit: Karnataka trip planner app"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/karnataka-trip-planner.git
git push -u origin main
```

## Deploy Live (GitHub Pages, Free)

1. Push your code to GitHub (steps above)
2. Repo page → Settings → Pages
3. Source: Deploy from branch → main → / (root) → Save
4. Wait 2–3 minutes
5. Your app is live at `https://YOUR_USERNAME.github.io/karnataka-trip-planner/`

If you're using Firebase, add this URL to authorized domains (Firebase Console → Authentication → Settings → Authorized domains).

## Project Structure

```
karnataka-trip-planner/
├── index.html          # Main app (all UI, logic, Firebase integration)
├── README.md           # This file
├── firebase-setup.md   # Step-by-step Firebase setup guide
└── .gitignore          # Files Git should ignore
```

Single-file architecture keeps it simple to demo, edit, and deploy. Production apps would split this into modules — but for a college project, the single-file approach is clearer.

## How the Itinerary Engine Works

For each destination, we score it against the user's preferences using a weighted scoring function:

```
score = base_score (from traveler type) 
      + interest_bonus (matches user-selected interests) 
      - walk_penalty (if elders mode and walk > threshold)
      - strenuous_penalty (if kids mode and place is strenuous)
```

Each traveler type has its own scoring key:
- **Couple** → uses `romantic` score
- **Family** → uses `family` score
- **Elders** → uses `elder_friendly` score (with walk cap)
- **Kids** → uses `kid_friendly` score (excludes strenuous spots)
- **Youth/Solo** → uses `romantic` score with higher spots-per-day

Top scored places are slotted into days based on pace (relaxed: 2/day, moderate: 3/day, packed: 4–5/day). Travel fatigue is calculated from average walking intensity, then displayed as a Low/Medium/High badge per day and overall.

For elders and kids, the engine injects rest/snack breaks between stops automatically.

## Customizing Destination Images

The app shows hero images for each destination in the Explore list and detail screen. Two images come pre-loaded (Mysuru and Hampi from Unsplash); the rest use beautiful gradient cards with the destination emoji as a fallback.

### To add real photos for more destinations:

1. Go to [unsplash.com](https://unsplash.com) or [pexels.com](https://pexels.com)
2. Search for a destination (e.g., "Coorg Karnataka")
3. Click any photo → right-click the image → **Copy image address**
4. Open `index.html`, find the `IMAGES` object near the top of the script section:

```js
const IMAGES={
  1:"https://images.unsplash.com/photo-1651569213711-b29d1fc3f995?w=600&q=80&auto=format&fit=crop",
  3:"https://images.unsplash.com/photo-1539413415627-9bd6cbc96518?w=600&q=80&auto=format&fit=crop"
};
```

5. Paste your URL using the destination ID:

```js
const IMAGES={
  1:"...",  // Hampi
  2:"YOUR_COORG_IMAGE_URL_HERE",  // Coorg
  3:"...",  // Mysuru
  // etc.
};
```

Destination IDs (1–25) follow the order in the `PL` array — Hampi=1, Coorg=2, Mysuru=3, Gokarna=4, Bandipur=5, and so on.

If an image URL ever breaks, the app automatically falls back to a colored gradient card with the destination emoji — so things never look broken.

## Customization Ideas

- Add more destinations to the `PL` array in `index.html`
- Adjust scoring weights in the `TC` (traveler config) object
- Modify budget allocation percentages (default: 35/35/20/10 for transport/stay/food/entry)
- Add new traveler types or interest categories
- Integrate Google Maps for actual routing between stops
- Add image uploads to user profiles

## License

MIT — feel free to use for your college project.
