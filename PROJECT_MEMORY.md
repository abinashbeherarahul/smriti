# Smriti Project Memory

## Project overview

Smriti is a browser-based brain-wellness companion for Anita. It provides a calm dashboard with daily reminders, brain games, caregiver support, encouragement, and accessibility controls.

## Current status

- The app has a React/Vite frontend and a Node.js/Express backend.
- Vite loads `index.html`, which mounts the React app from `src/main.jsx`.
- Progress and care-routine completion are stored locally in browser `localStorage`.
- Browser speech synthesis is used for short local encouragement messages.
- The Talk with Smriti glass button opens the Smriti AI assistant in a compact browser window.
- The backend uses PostgreSQL for accounts, friends, friend requests, reminders, and game progress.
- The target full-stack architecture and security requirements are documented in the root Markdown files.

## Features

- My day dashboard
- Brain games with beginner, advanced, and pro levels
- Beginner Colourful pairs memory game
- Beginner grid-based jigsaw puzzle with gallery, difficulty selection, hints, reference view, and completion feedback
- Patient profile panel with name, age, place, and patient ID
- Friends panel with friend count, active/offline indicators, and Friend ID search
- Friend-request notification panel
- Daily reminders and completion tracking
- Caregiver routine checklist
- Caregiver notification and messaging simulations
- Mood check-in
- Emergency-support confirmation flow
- Rotating inspirational quotes
- Baby character interaction and baby-babble audio
- Welcome/onboarding modal

## Important files

- `src/main.jsx` - React entry point
- `src/App.jsx` - View routing
- `src/components/` - Part-by-part JSX screens
- `styles.css` - Visual design and responsive layout
- `games/beginner/ColorPairs.jsx` - Memory Garden matching game
- `games/beginner/Puzzle.jsx` - Gallery and grid-based jigsaw puzzle
- `src/components/Layout.jsx` - Navigation, profile, friends, notifications, and assistant controls
- `backend/src/app.js` - Express API composition without opening a port
- `backend/src/server.js` - Backend startup and graceful shutdown
- `backend/src/db/migrations/001_initial.sql` - PostgreSQL schema migration
- `public/baby-character.png` - Character artwork
- `public/baby-babble.mp3` - Baby-babble audio
- `README.md` and the stack-specific Markdown files - Full-stack architecture, API, authentication, version-control, and security requirements

## How to run

Install dependencies and start the Vite development server:

```text
npm install
npm run dev
```


## Development notes

- Keep the frontend responsive and usable while the backend is unavailable.
- Use the backend for persistent account, profile, friend, notification, reminder, and game-progress data.
- Preserve accessible labels, keyboard interaction, and visible focus states.
- Keep user progress local to the device.
- Keep patient and friend data clearly separated from future server-backed account data.
- Preserve the compact popup behavior for profile, friends, and notifications when updating the top bar.
