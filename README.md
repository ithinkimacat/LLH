# Large Language Human

**The first AI assistant powered entirely by human intelligence.**

A social experiment where visitors collectively construct a sentence, one word at a time, through consensus voting. When enough people vote for the same word, it gets published and the next round begins.

## How It Works

1. Visit [largelanguagehuman.com](https://largelanguagehuman.com)
2. Read the prompt and submit a word
3. Your word appears as a candidate — others can vote for it
4. When a candidate reaches the threshold (default: 5 votes), it auto-publishes
5. Queue clears. Everyone votes for the next word

## Tech Stack

- **Frontend**: Static HTML/CSS/JS on GitHub Pages
- **Backend**: Firebase Realtime Database + Anonymous Auth
- **Consensus**: Client-side `runTransaction` race-lock
- **Real-time**: Firebase `onValue` listeners

## Features

- Anonymous auth — no signup required
- Real-time candidate voting with live vote counts
- Click any candidate to vote, click again to cancel
- Delete-last-word consensus button
- Dark/light theme toggle
- Connection status indicator
- Persistent participant counter

## Local Testing

```bash
python3 -m http.server 8000
```

Open http://localhost:8000

## Created by

Alex Polin — [LinkedIn](https://www.linkedin.com/in/alex-polin/)
