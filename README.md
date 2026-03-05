# Discipline — Behavioural Tracker

A personal chastity and behaviour tracking web app. Tracks daily lock status, streaks, orgasm events, and a range of activities. Clean UI, data visualisation, and a sardonic AI keyholder personality called The Warden.

Self-hosted — your data lives in your own Firebase account and never touches anyone else's infrastructure.

---

## Features

- Daily lock status logging with calendar view
- Streak tracking — current, record, lifetime locked
- Orgasm logging and denial tracking
- Activity logging (configurable)
- Year heatmap, rolling trend chart, monthly breakdown
- Pinned stats — customise your dashboard
- The Warden — a sardonic daily message based on your stats
- Warden page — data snapshot export and Claude Project integration
- Dark and light theme
- CSV export
- Bulk historic period entry
- PWA — installable on mobile

---

## Setup

This app uses Firebase for authentication and data storage. You'll need a free Firebase account.

### 1. Create a Firebase project

1. Go to [firebase.google.com](https://firebase.google.com) and sign in with a Google account
2. Click **Add project** and follow the steps (you can disable Google Analytics)
3. Once created, click the **web icon** (`</>`) to add a web app
4. Give it a name, optionally enable Firebase Hosting, then click **Register app**
5. Copy the `firebaseConfig` object that appears — you'll need it in a moment

### 2. Enable Authentication

1. In the Firebase console, go to **Build > Authentication**
2. Click **Get started**
3. Under **Sign-in method**, enable **Email/Password**

### 3. Enable Realtime Database

1. Go to **Build > Realtime Database**
2. Click **Create database**
3. Choose a region (Europe recommended for UK/EU users)
4. Start in **locked mode** (you'll set rules next)

### 4. Set database rules

In the Realtime Database console, go to the **Rules** tab and replace the contents with:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```

Click **Publish**.

### 5. Add your Firebase config to the app

Open `discipline-tracker.html` in a text editor. Find this section near the top of the `<script>` block:

```javascript
const firebaseConfig={
  apiKey:"YOUR_API_KEY",
  authDomain:"YOUR_PROJECT_ID.firebaseapp.com",
  ...
};
```

Replace the placeholder values with the values from your Firebase project's config object.

### 6. Run the app

You can run it in two ways:

**Locally** — just open `discipline-tracker.html` in a browser. Most things will work, but some browser security restrictions may apply depending on your setup.

**Via Firebase Hosting (recommended)** — free static hosting from Firebase.

1. Install the [Firebase CLI](https://firebase.google.com/docs/cli): `npm install -g firebase-tools`
2. In a terminal, run `firebase login`
3. Create a folder for your project, put `discipline-tracker.html` inside it renamed as `index.html`
4. Run `firebase init hosting` in that folder and follow the prompts
5. Run `firebase deploy`
6. Your app will be live at `https://YOUR_PROJECT_ID.web.app`

---

## The Warden

The Warden is a sardonic keyholder personality built into the app. There are two parts to it.

### Dashboard messages

A daily message appears on your dashboard, selected based on your current streak, days since last orgasm, and other stats. These messages are baked directly into the app — no account or API needed, they just work.

### Interactive Warden (optional)

The full interactive Warden lives in a Claude Project — a persistent AI conversation that knows your history, tracks a hidden token balance, issues assignments, and controls access to privileges. It requires a free or paid [Anthropic account](https://claude.ai).

#### First-time setup — Initiation

You only do this once.

1. Log some data first so the Warden has something to work with
2. In the app, go to the **Warden** tab
3. Click **Copy Initiation Prompt**
4. Go to [claude.ai](https://claude.ai) and create a new **Project** (not a regular conversation — Projects maintain memory across sessions)
5. Paste the initiation prompt as your first message and send it
6. The Warden will conduct a short onboarding process — answer its questions honestly. It will ask about hard limits, scope, and expectations. Take your time with this.
7. Once onboarding is complete, the system is live

#### Every subsequent session

1. In the app, go to the **Warden** tab
2. Click **Copy Snapshot** — this generates a plain text summary of all your current stats
3. Open your Warden Claude Project
4. Paste the snapshot at the start of your message, then say whatever you need to say or make your request
5. The Warden will respond in character with full knowledge of your current data

The snapshot is important — Claude Projects don't read your Firebase data directly, so pasting the snapshot is how the Warden stays informed. Don't skip it.

#### Key phrases

| Phrase | Effect |
|---|---|
| `Warden, administrative note:` | Drops the persona. Use this to discuss mechanics, adjustments, or anything out of character. |
| `Resume` | Returns to character after an administrative discussion. |
| `The watch is over` | Ends the dynamic entirely. The Warden will give a closing statement. History is preserved if you return. |

#### How it works

The Warden maintains a hidden token balance that accrues while you're locked and denied, and depletes when you unlock or orgasm. Erotic activities — edging, plug use, and others — are gated behind privilege requests which cost tokens. The Warden may also issue assignments. Your visible rank reflects cumulative behaviour over time. None of the internal mechanics are exposed directly — you interact with the Warden in natural language and it manages everything behind the scenes.

---

## Data & Privacy

All your data is stored in your own Firebase Realtime Database under your own Google account. Nobody else has access to it. The app makes no external requests except to Firebase (for your data) and Google Fonts (for typography).

---

## License

MIT — do whatever you like with it.
