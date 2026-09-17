# Smriti technology and development report

**Inventory date:** 12 September 2026  
**Project type:** React/Vite client with an Express/PostgreSQL API

## 1. Executive summary

The working website is a React/Vite client backed by an Express API and PostgreSQL persistence. Authentication uses short-lived bearer access tokens plus an HTTP-only refresh cookie. The UI retains a graceful demo mode when the API is unavailable or the visitor is signed out.

The React, Vite, Node.js, Express, PostgreSQL, REST, JWT/session, and security documents describe the current full-stack implementation. Some product areas, including caregiver-specific persistence and advanced/pro games, remain staged for later work.

## 2. Technologies actually used

### Markup

- HTML5 document structure and semantic elements such as `main`, `nav`, `aside`, `header`, `section`, `button`, `form`, `label`, `textarea`, and `audio`.
- Responsive viewport metadata.
- Accessibility attributes including `aria-label`, `aria-live`, `aria-expanded`, `aria-selected`, `role`, and a skip link.
- React/Vite entry point:
  - `src/main.jsx` mounts the application.
  - `src/App.jsx` switches between dashboard screens and games.

### Styling

- CSS3 in `styles.css`.
- CSS custom properties for the color and spacing design system.
- Flexbox and CSS Grid for layouts.
- Media queries for mobile layouts.
- CSS transitions, focus-visible outlines, rounded cards, shadows, gradients, and decorative illustrations.
- External Google Fonts:
  - DM Sans, weights 400/500/600/700.
  - Fraunces, weights 600/700.

### JavaScript

- React 18 components with Vite.
- Dashboard screens are split into `src/components/Home.jsx`, `Games.jsx`, `Routine.jsx`, and `CareCircle.jsx`.
- Shared navigation and shell behavior live in `src/components/Layout.jsx`.
- Component state and event handlers replace direct DOM manipulation.
- JSON serialization for transient demo state; authenticated reminders and game progress are persisted by the API.
- Timers with `setTimeout`, `setInterval`, and `clearInterval`.
- Date formatting with `Intl.DateTimeFormat("en-IN", ...)`.

## 3. Browser APIs and platform capabilities

- Backend API persistence for reminders and game progress; signed-out demo state is intentionally transient.
- Web Speech API:
  - `speechSynthesis` and `SpeechSynthesisUtterance` for spoken encouragement.
  - `SpeechRecognition` / `webkitSpeechRecognition` for optional voice reminder entry.
- Web Audio API:
  - `AudioContext` / `webkitAudioContext`.
  - Oscillators and gain nodes for a fallback baby sound.
- HTML audio playback for `baby-babble.mp3`.
- `window.open()` for the external “Talk to Smriti” AI window.
- `window.confirm()` for the emergency-contact confirmation flow.
- `scrollIntoView()` and `scrollTo()` for navigation behavior.
- `pagehide` for stopping audio when leaving the page.

Browser support is therefore dependent on speech recognition, speech synthesis, audio, pointer, and local-storage support. Unsupported optional capabilities receive an in-app fallback message.

## 4. Features implemented

- Calm wellness dashboard.
- Daily reminders with completion toggles.
- Add-reminder form with optional voice input.
- Beginner, advanced, and pro game categories.
- Caregiver routine checklist.
- Simulated caregiver notification, call, and message flows.
- Mood check-in with spoken feedback.
- Emergency support confirmation simulation.
- Text-size controls.
- Rotating inspirational quotes.
- Baby character interaction and audio.
- Welcome/onboarding modal.
- Responsive navigation and mobile menu.
- Visible keyboard focus styles and live status messages.

## 5. Project assets

| Asset | Purpose |
| --- | --- |
| `baby-character.png` | Baby/companion artwork |
| `baby-babble.mp3` | Baby-babble audio |


## 6. External resources and services

- Google Fonts is loaded from `fonts.googleapis.com` and `fonts.gstatic.com`.
- The “Talk to Smriti” button opens an external hosted AI page:
  `https://ais-dev-q7ulm7n3gu6sdydu4jrovg-243079986760.asia-east1.run.app/`
- No application API calls, database calls, payment service, analytics SDK, CDN JavaScript library, or cloud storage integration is present in the local source.
- Speech recognition behavior may depend on browser-provided services; the project does not configure a speech provider itself.

## 7. Development and deployment technology

### Development commands

- `npm install` installs frontend and backend dependencies.
- `npm run dev` starts the Vite development server.
- `npm run build` creates the production frontend bundle.
- `npm run backend:migrate` applies PostgreSQL migrations.
- `npm run backend` starts the API server.

### Implemented backend integration

The backend exposes REST endpoints for authentication, profile, friends, notifications, reminders, and game progress. Care-circle contact details remain intentionally modeled as unavailable until a caregiver API exists.
- Production security controls such as server-side authorization, rate limiting, migrations, secret management, and request timeouts are present. CSRF protection, encrypted database storage, and security CI still require deployment-specific configuration.

## 8. Security status

The app has authenticated server-backed accounts, short-lived access tokens, HTTP-only refresh cookies, server-side validation, authorization, rate limiting, and security headers. Important remaining limitations include:

- Caregiver contact details and care-circle actions remain demo UI until caregiver-specific endpoints are added.
- The external AI URL is opened directly by the browser.
- User-uploaded images are local browser object URLs and are not uploaded to the project backend.
- CSRF protection for cookie-authenticated refresh/logout routes and deployment-specific database access controls should be added before production launch.

Use [SECURITY.md](./SECURITY.md) as the target security baseline before production deployment.

## 9. Run instructions

Copy `.env.example` to the backend environment, configure `DATABASE_URL`, run migrations, then start the API with `npm run backend` and the client with `npm run dev`. `npm run build` validates the production SPA bundle. Vite proxies `/api` locally; configure `VITE_API_URL` when deploying separately. The Colourful pairs game is reachable at `/beginner-games/color-pairs`.
