
# ID Chat — React + Firebase real-time backend

This version is the real backend version of the previous frontend demo.

## What it does
- Firebase Authentication
- Firestore real-time 1-to-1 chat
- Unique 6-digit user ID
- Search user by ID
- Profile/profile photo
- Online/offline field
- Block/unblock
- Delete own message
- Firebase Storage image/file upload (5 MB demo limit)
- Web push notification scaffolding with Firebase Cloud Messaging
- Responsive mobile/web UI

## IMPORTANT: "SMS" vs app notification
This project sends a REAL-TIME CHAT MESSAGE through Firebase and can show a PUSH NOTIFICATION on the other phone/browser.

It does NOT send a cellular SMS to a phone number. Cellular SMS requires an SMS provider such as Twilio/Vonage and a phone-number based backend.

## Firebase setup
1. Create a Firebase project.
2. Add a Web App.
3. Enable Authentication -> Email/Password.
4. Create Firestore Database.
5. Create Storage.
6. Put the Firebase web config into `.env` using `.env.example`.
7. In Firebase Console -> Cloud Messaging, create/generate a Web Push certificate (VAPID key) and put the public key in `VITE_FIREBASE_VAPID_KEY`.
8. Deploy the included `firebase-messaging-sw.js` at the site root and replace its placeholder Firebase config.
9. Paste the included `firestore.rules` into Firestore Rules and publish.
10. Run `npm install` then `npm run dev`.
11. For Tiiny/static hosting run `npm run build`, then upload `dist` contents.

## Notification requirement
Web push needs HTTPS and browser notification permission. Firebase documents FCM Web support and its service-worker requirements:
https://firebase.google.com/docs/cloud-messaging/web/get-started

## One important backend step not included
For a production chat notification, when User A writes a message, a trusted server/Cloud Function should look up User B's FCM token and send an FCM notification. Do NOT put Firebase Admin credentials in React/browser code.

Firestore provides real-time listeners so both connected clients can see new messages without refreshing.

## Production security
The included rules are a starter and should be reviewed before production. Add stronger validation for message fields, membership, file metadata, profile changes, rate limits, abuse controls, and group-chat authorization.

End-to-end encryption is NOT implemented in this starter.

## Login/profile fix
This version keeps Firebase Auth login independent from Firestore profile loading. After authentication it loads/repairs the `uidMap` and `users/{6-digit-id}` profile, so the app does not remain stuck on the login screen when the Auth account exists.

If you replace this version, keep your own `.env` file. `.env` is intentionally not included in the ZIP.
