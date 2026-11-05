# Skin Care Routine

Jonathan and Monique's daily skin care app: today's steps, mask rotation, product usage,
a daily log, and packing lists for trips.

Live site: https://jskagan.github.io/skin-care/

## How it is put together

- **One page, no build.** `index.html` holds everything. `img/` has the product photos and the
  app icon, `pdf/` the printable routines.
- **Sign-in.** Google sign-in through Firebase Auth. Only the owner (jskagan@gmail.com) and the
  emails listed in Firestore doc `skincare_config/members` can use the app. The owner manages that
  list from the footer of the app ("Who can use this app").
- **Shared data.** Firestore collections `skincare_checks`, `skincare_bottles`, `skincare_trips`,
  `skincare_config`. **These live in the `bowl-donor-chart` Firebase project, shared with the
  Hollywood Bowl donor chart**, to keep one project to manage. Everything this app stores starts
  with `skincare_`, so it can be moved to its own project later by changing `FIREBASE_CONFIG` in
  `index.html`, deploying `firestore.rules` there, and copying those collections.
- **Rules.** `firestore.rules` here is the reference copy. A Firebase project has one rules file, so
  the `skincare_` block must be pasted into the donor chart repo's `firestore.rules` and deployed
  with `firebase deploy --only firestore:rules` from that repo.
- **Trips.** Trip planning can read `trips-data.json` from Google Drive, the same file Travel
  Tracker (jskagan/Flight-data) writes. It uses Travel Tracker's Google OAuth client, which works
  because both apps are served from `jskagan.github.io`. Anyone using that button needs the Drive
  file shared with their Google account.
- **PDFs.** Packing-list PDFs are made in the browser with jsPDF (cdnjs). Routine PDFs are the
  static files in `pdf/`.

## One-time setup (Firebase console, project bowl-donor-chart)

1. Authentication → Settings → Authorized domains → add `jskagan.github.io`.
2. Authentication → Sign-in method → Google must be enabled (it already is for the donor chart).
3. Deploy the Firestore rules as described above.

## Changing the routine

Products, order, mask rotation and the mask start date are the constants at the top of the script
in `index.html` (`SHARED`, `HER_SHARED`, `MASKS`, `START`, `EVERY`). After changing products,
regenerate the PDFs in `pdf/`.
