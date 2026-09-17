# Smriti web application workflow

This diagram describes the current application flow from browser startup through navigation, authentication, page interactions, API calls, and responsive support controls.

## Complete application flow

```mermaid
flowchart TD
    A[User opens Smriti URL] --> B[Vite serves React application]
    B --> C[main.jsx mounts App and AuthProvider]
    C --> D[AuthProvider calls POST /api/v1/auth/refresh]
    D --> E{Refresh succeeds?}
    E -->|Yes| F[Store access token and user]
    E -->|No| G[Continue in demo mode]
    F --> H[App reads window.location.pathname]
    G --> H

    H --> I{Route}
    I -->|/| J[Home - My day]
    I -->|/beginner-games| K[Games]
    I -->|/beginner-games/puzzle| L[Puzzle]
    I -->|/beginner-games/color-pairs| M[Colourful pairs]
    I -->|/routine| N[My routine]
    I -->|/caregiver| O[Care circle]
    I -->|Unknown path| J

    J --> P[Shared Layout]
    K --> P
    N --> P
    O --> P
    P --> Q[Sidebar navigation]
    P --> R[Topbar controls]
    P --> S[Talk with Smriti assistant]
    P --> T[Need Help support link]

    Q -->|My day| J
    Q -->|Brain games| K
    Q -->|My routine| N
    Q -->|Care circle| O

    R --> R1[Friends panel]
    R --> R2[Notifications panel]
    R --> R3{Authenticated?}
    R3 -->|Yes| R4[Profile panel and Sign out]
    R3 -->|No| R5[Open authentication flow]

    P --> U{Authenticated user?}
    U -->|No| V[Local UI state and demo progress]
    U -->|Yes| W[Load account data through API]
    W --> W1[Friends]
    W --> W2[Notifications]
    W --> W3[Reminders]

    J --> J1[Choose an activity]
    J1 --> K
    J --> J2[See routine]
    J2 --> N
    J --> J3[Open my routine]
    J3 --> N
    J --> J4[Play a game]
    J4 --> K
    J --> J5[Open care circle]
    J5 --> O
    J --> J6[Cycle inspirational quote]
    J6 --> J

    K --> K1[Choose Beginner, Advanced, or Pro]
    K1 --> K2{Level available?}
    K2 -->|Beginner| K3[Show Colourful pairs and Jigsaw puzzle]
    K2 -->|Advanced or Pro| K4[Show locked or gated experience]
    K3 --> K5[Play now]
    K3 --> K6[Start game]
    K5 --> M
    K6 --> L
    M --> M1[Play memory matching game]
    M1 --> M2[Update local game state]
    L --> L1[Arrange puzzle pieces]
    L1 --> L2[Update canvas and puzzle state]
    M2 --> M3{User signed in?}
    L2 --> L3{User signed in?}
    M3 -->|Yes| M4[Save game progress]
    L3 -->|Yes| L4[Save game progress]
    M3 -->|No| M5[Keep progress locally]
    L3 -->|No| L5[Keep progress locally]
    M4 --> G1[PUT /api/v1/games/:gameId/progress]
    L4 --> G1

    N --> N1[Load reminders]
    N1 --> N2{Authenticated user?}
    N2 -->|Yes| N3[GET /api/v1/reminders]
    N2 -->|No| N4[Use default reminders]
    N3 --> N5[Render synced reminders]
    N4 --> N5
    N5 --> N6[Complete or uncomplete reminder]
    N6 --> N7{Authenticated reminder?}
    N7 -->|Yes| N8[PATCH /api/v1/reminders/:id]
    N7 -->|No| N9[Update local state]
    N5 --> N10[Choose mood]
    N5 --> N11[Open Add reminder form]
    N11 --> N12[Validate title and time]
    N12 --> N13{Valid input?}
    N13 -->|No| N14[Keep form open and show validation]
    N13 -->|Yes and signed in| N15[POST /api/v1/reminders]
    N13 -->|Yes and demo mode| N16[Append reminder locally]
    N15 --> N17[Render new reminder]
    N16 --> N17
    N8 --> N18[Update progress and Next up]
    N9 --> N18
    N17 --> N18

    O --> O1[Render Meera contact card]
    O1 --> O2[Call Meera]
    O1 --> O3[Send a note]
    O --> O4[Render 9-step care schedule]
    O4 --> O5[Tap completion control]
    O5 --> O6[Toggle completed Set]
    O6 --> O7[Recalculate completed count]
    O7 --> O8[Update progress bar]
    O8 --> O9[Update remaining steps]
    O9 --> O10[Update Next up]
    O10 --> O11{All 9 complete?}
    O11 -->|No| O4
    O11 -->|Yes| O12[Show complete care-plan message]

    S --> S1[Open external Smriti assistant window]
    T --> T1[Open telephone support link]

    W --> X[API request wrapper]
    X --> X1[Attach bearer token and cookies]
    X1 --> X2[Send request to Express backend]
    X2 --> X3{HTTP response}
    X3 -->|200-299| X4[Return JSON to React]
    X3 -->|401| X5[Attempt one token refresh]
    X5 --> X6{Refresh succeeds?}
    X6 -->|Yes| X7[Retry original request once]
    X6 -->|No| X8[Continue unauthenticated or show error]
    X3 -->|Other error| X9[Throw API error]
    X9 --> X10[Page shows sync or action error]

    X2 --> Y[Express middleware]
    Y --> Y1[Request ID]
    Y1 --> Y2[Helmet security headers]
    Y2 --> Y3[CORS]
    Y3 --> Y4[JSON body parser]
    Y4 --> Y5[Cookies and timeout]
    Y5 --> Y6[Rate limiter]
    Y6 --> Y7[Route handler]
    Y7 --> Y8[Validation and auth middleware]
    Y8 --> Y9[Repository]
    Y9 --> Y10[PostgreSQL]
    Y10 --> Y11[JSON response]
    Y11 --> X4

    Z[Browser width changes] --> Z1{Viewport}
    Z1 -->|Desktop| Z2[Fixed sidebar and multi-column content]
    Z1 -->|Tablet| Z3[Reduced spacing and flexible columns]
    Z1 -->|Mobile| Z4[Menu button, stacked cards, touch-friendly buttons]
    Z4 --> Z5[Main content remains vertically scrollable]
```

## Page-level interaction summary

| Area | Main user actions | State or persistence |
| --- | --- | --- |
| My day | Choose routine, games, or care circle; cycle quote | Navigation and quote are local UI state |
| Brain games | Select level; open Colourful pairs or Jigsaw puzzle | Game state is local; signed-in progress can be saved |
| My routine | Complete reminders, choose mood, add reminders | Demo mode is local; signed-in reminders use the API |
| Care circle | Call Meera, send note, complete nine care steps | Completion state is local to the page session |
| Friends | Search connected friends | Loaded from `/friends` for authenticated users |
| Notifications | Accept or decline friend request | Current request decision is local UI state |
| Profile | View profile or sign out | Authentication context and `/auth/logout` |
| Need Help | Open telephone support | External device/browser telephone action |
| Talk with Smriti | Open assistant | External assistant window |

## Authentication flow

```mermaid
flowchart LR
    A[App start] --> B[POST /auth/refresh]
    B --> C{Valid refresh cookie?}
    C -->|Yes| D[Receive access token and user]
    C -->|No| E[Set loading false and remain in demo mode]
    D --> F[Authenticated API requests]
    E --> G[Local-only interactions]
    H[Sign in or Register] --> I[POST /auth/login or /auth/register]
    I --> J{Credentials valid?}
    J -->|Yes| D
    J -->|No| K[Show authentication error]
    L[Sign out] --> M[POST /auth/logout]
    M --> N[Clear token and user state]
```

## Error and fallback paths

1. An unavailable refresh request does not block the interface; the application remains usable in demo mode.
2. A protected API request receiving `401` tries one refresh request and then retries the original request once.
3. Failed reminder synchronization is shown through the routine page error state.
4. Unknown client-side paths fall back to the My day view.
5. Empty friends or notifications responses render friendly empty states.
6. Narrow screens use the menu button and stacked layouts instead of horizontal scrolling.

