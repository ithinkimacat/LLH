# Large Language Human

A social experiment where every visitor anonymously submits 2-3 characters to build a crowd-sourced sentence, displayed in a ChatGPT-like UI.

## Architecture

- **Frontend**: Static HTML/CSS/JS served by GitHub Pages
- **Backend**: Firebase Realtime Database + Anonymous Auth
- **Real-time**: Firebase `onValue` listeners update all clients instantly
- **Cooldown**: 5-second per-user enforced by database security rules

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
   - Replace `YOUR_API_KEY`, `YOUR_PROJECT_ID`, etc. with values from step 3

5. **Deploy to GitHub Pages**
   - Push repo to GitHub
   - Settings → Pages → Source: deploy from branch `main`, folder `/` (root)

6. **Set question**
   - In Firebase console, add node `sentence/question` with your prompt string
   - Update anytime without redeploying

## Database Structure

```
sentence/
  question: "Your prompt here"
  tokens/
    -Nabc...: { text: "Th", authorId: "uid...", timestamp: ServerValue.TIMESTAMP }
  lastSubmit/
    uid...: 1715070000000
```

## Security Rules Summary

- Tokens: authenticated users only, 2-3 chars, must match auth UID, no overwrites
- `lastSubmit`: 5-second cooldown enforced server-side
- `question`: read-only from client (set manually in console)

## Local Testing

Serve locally with any static server:
```bash
python3 -m http.server 8000
```
Open http://localhost:8000
