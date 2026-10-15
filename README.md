# FF Arena XP Calculator

Separate mobile-first PWA for tracking Free Fire BR/CS ranks and XP. This version uses Firebase Authentication and Cloud Firestore in project `ff-xp-calculater`.

## Access model
- Admins can create/edit/delete players, award XP, change rank rewards and export backups.
- Signed-in players can view the shared leaderboard, player records and history.
- Accounts are created in Firebase Authentication by the owner/admin. There is no public sign-up.
- Every account needs a matching Firestore document at `users/{AUTH_UID}` with `role` set to `admin` or `player`.
- Never let users edit their own role document. Firestore rules deny all client writes to `users`.

## Setup
1. Upload/replace all files from this project in the NEW GitHub repository `ff-arena-xp-calculator`.
2. In Firebase Console for `ff-xp-calculater`, open Firestore Database > Rules and publish the contents of `firestore.rules`.
3. In Firebase Authentication > Sign-in method, enable Email/Password.
4. In Authentication > Users, add the admin account (email/password). Copy its **UID**.
5. In Firestore Data, create collection `users`, document ID exactly equal to that UID, with field `role` (string) = `admin`.
6. Add player accounts in Authentication and create `users/{their UID}` documents with `role` (string) = `player`.
7. Commit the files to GitHub. Railway should redeploy the separate XP Calculator service. Do not change the original FF Arena service or Firebase project `ffarena-4f773`.
8. Open the XP Calculator URL and sign in as the admin. Add a fake test player, refresh, and verify the record remains. Sign in with a player account in another browser/private window and verify edit controls are absent.

## Cloud data
The app stores shared records in `appData/main`. If that document does not exist, the first admin login initializes it from that browser's local backup if present. Keep an exported backup before testing. Existing browser-local data is not automatically merged into an existing cloud document.

## Important security notes
- Firebase web config is designed to be included in a web app; Firestore rules and Authentication protect the data. Do not publish service-account private keys.
- `firestore.rules` permits signed-in users to read the shared leaderboard. If player UID/privacy requirements change, revise the read policy before entering real data.
