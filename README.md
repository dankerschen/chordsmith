# Fretwise — guitar chord, progression & scale trainer

A single-page guitar learning tool. Live at <https://dankerschen.github.io/fretwise/>.

- **Chord charts** — chord finder for any root and type, with CAGED shapes, arpeggio map and triad inversions across the neck.
- **Progressions** — common progressions in any key, strumming patterns, capo (manual or automatic), a transposer with capo suggestions, and a backing track (drums + bass) with the scale to solo with.
- **Scales** — scales and the seven modes on the fretboard.
- **Practice** — chord-name quiz and one-minute chord changes.
- **Drill** — random chords, scales, progressions, fretboard notes and ear training (chord types, intervals, chord changes), with weak spots coming up more often.
- **Tuner** — microphone tuner with standard and alternate tunings.
- **Progress** — practice streak, calendar, drill results and chord-change scores over time.

Open `index.html` in a browser to use it; progress is then saved in that browser. On the live site it can be installed as an app (📲 **Install** in the header, or **Share → Add to Home Screen** on iPhone) and works offline. The tuner needs the live site or the installed app, because browsers only allow the microphone over https.

The site files are `index.html`, `firebase-config.js`, `manifest.webmanifest`, `sw.js` (offline support) and the `icons/` folder.

To let friends use it from anywhere, each with their own saved progress, host it on GitHub Pages and connect a free Firebase project. One-time setup, about 15–20 minutes.

## 1. Put it on GitHub Pages

No git needed, everything happens on the GitHub website.

1. Sign in at <https://github.com>, click **+ → New repository**, name it e.g. `guitar-tool`, set it to **Public**, tick **Add a README file**, and click **Create repository**.
2. In the new repository click **Add file → Upload files**. Drag in `index.html`, `firebase-config.js`, `firestore.rules` and `README.md` from this folder. Click **Commit changes**; it's fine to replace the README GitHub created.
3. Go to **Settings → Pages → Build and deployment**. Choose **Deploy from a branch**, branch `main`, folder `/ (root)`, then **Save**.
4. After a minute or two the site is live at `https://<your-username>.github.io/guitar-tool/`. The Pages settings page shows the link once it's ready.

**Updating later:** after changing a file here, upload it the same way (**Add file → Upload files**, then **Commit changes**). It replaces the old version, and the site updates within a couple of minutes. To edit a small thing like `firebase-config.js` directly on GitHub, open the file, click the pencil icon, edit, and commit.

## 2. Create the Firebase project (accounts + storage)

1. Go to <https://console.firebase.google.com>, click **Create a project**. Google Analytics isn't needed.
2. **Add a web app**: on the project overview click the `</>` (Web) icon, give it a name. Hosting isn't needed. Firebase shows a `firebaseConfig = { ... }` object.
3. Copy that object into `firebase-config.js`, replacing `null`:
   ```js
   window.FIREBASE_CONFIG = { apiKey: "...", authDomain: "...", projectId: "...", storageBucket: "...", messagingSenderId: "...", appId: "..." };
   ```
   These values are safe to commit. They only identify the project; the database rules below decide who can read what.
4. **Turn on sign-in**: *Build → Authentication → Get started → Sign-in method*, enable **Google** and **Email/Password**.
5. **Allow your site**: *Authentication → Settings → Authorized domains → Add domain*, enter `<your-username>.github.io`.
6. **Create the database**: *Build → Firestore Database → Create database*. Pick a location near you and start in **production mode**.
7. **Set the rules**: in Firestore, open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**. This makes each person's progress private to them.
8. Upload the updated `firebase-config.js` to the repository, or edit it on GitHub with the pencil icon. The **Sign in** button appears on the live site.

The free Spark plan covers this comfortably: up to 50k document reads and 20k writes a day.

## How saving works

- **Not signed in:** progress is saved in that browser only.
- **Signed in:** drill ratings, best chord-change scores and the practice log (for the Progress tab) are saved to the person's account and follow them to any device. Settings such as theme and note names stay per device.
- On the first sign-in in a browser, the tool offers to add any progress already made there to the account.
- **Offline:** a signed-in person's progress is kept on the device and synced when the connection returns. If it can't be loaded at all, progress is saved in the browser and offered to the account next time, so the cloud copy is never overwritten.
- Signing out on a shared computer leaves no account data behind in that browser.

## Testing sign-in

Sign-in doesn't work when `index.html` is opened directly as a file; it then saves to that browser only, with no Sign in button. Test accounts on the live GitHub Pages site instead. Opening the local file is still fine for checking everything else before you upload.
