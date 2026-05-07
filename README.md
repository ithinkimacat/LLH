# Large Language Human

A social experiment where visitors collectively construct a sentence, one word at a time, through consensus voting. When enough people vote for the same word, it gets published and the next round begins.

## Architecture

- **Frontend**: Static HTML/CSS/JS served by GitHub Pages
- **Backend**: Firebase Realtime Database + Anonymous Auth
- **Consensus**: Client-side `runTransaction` race-lock — when a candidate reaches threshold, all clients race to publish; one wins, queue clears
- **Real-time**: Firebase `onValue` listeners update all clients instantly

## How It Works

1. Visitor anonymously authenticates via Firebase
2. Submits a word (1-20 characters) as a vote
3. Word appears as a candidate. Others can vote for the same word (case-insensitive)
4. When a candidate reaches the threshold (default: 5 unique voters), it auto-publishes to the sentence
5. Queue clears. Everyone can vote for the next word

## Setup

1. **Create Firebase project**
   - Go to [Firebase Console](https://console.firebase.google.com)
   - Create project, enable Realtime Database, enable Anonymous Auth

2. **Apply security rules**
   - In Database Rules, paste contents of `firebase-rules.json`

3. **Get config**
   - Project Settings → General → Your apps → Web app
   - Copy config object

4. **Plug config into app**
   - Open `index.html`
   - Replace the `firebaseConfig` object with your real values (already done if you see real keys)

5. **Deploy to GitHub Pages**
   - Push repo to GitHub
   - Settings → Pages → Source: deploy from branch `main`, folder `/` (root)

6. **Set question and threshold**
   - In Firebase console, Realtime Database → Data tab
   - Add:
     ```json
     {
       "sentence": {
         "question": "Your prompt here",
         "threshold": 5,
         "words": {},
         "candidates": {},
         "publishState": null
       }
     }
     ```
   - Update `question` and `threshold` anytime without redeploying

## Database Structure

```
sentence/
  question: "Your prompt here"
  threshold: 5
  words/
    -Nabc...: { text: "hello", timestamp: ServerValue, authorCount: 5 }
  candidates/
    hello: { count: 3, voters: { uid1: true, uid2: true, uid3: true }, text: "Hello" }
  publishState: { word: "hello", text: "Hello", timestamp: 12345 }
```

## Security Rules Summary

- `words`: authenticated users only, requires text + timestamp + authorCount
- `candidates`: authenticated users only, one vote per user per word, no duplicate voters
- `publishState`: authenticated users only, used for race-lock publishing
- `question` / `threshold`: admin only (set manually in console)

## Local Testing

```bash
python3 -m http.server 8000
```
Open http://localhost:8000
