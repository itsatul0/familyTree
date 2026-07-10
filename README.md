# Family Tree Explorer

A shared multi-user family tree visualizer built with React 18, D3.js and Firebase Firestore. The attached 79-member tree rooted at **Pradhuman Lal das** is embedded as the first-run seed.

## Included

- D3 SVG tree with pan, zoom, fit, generation colors and dark mode
- Search, generation drawer, collapse/expand and ancestor/descendant highlighting
- Add, edit and delete member flows with validation
- JSON import by file, drag/drop or raw editor, plus JSON and PNG export
- 40-action client undo/redo history
- Per-browser editor name capture and editable identity
- Firestore single-document persistence at `familyTrees/main`
- Firestore real-time `onSnapshot` sync, sync state and last-editor attribution
- Remote updates queued while add/edit or JSON UI is open
- Firebase Hosting config and GitHub Actions deployment workflow

## 1. Create Firebase project

1. Create a Firebase project.
2. Create a Firestore database.
3. Add a Web App in Firebase Project Settings.
4. Copy the Firebase web config into `public/firebase-config.js`.
5. Copy `.firebaserc.example` to `.firebaserc` and replace the project id.

The included Firestore rule intentionally allows public read/write because the requested app has no authentication. Anyone with the public URL can edit the tree. For a private family tree, add Firebase Authentication and tighten the rule before sharing broadly.

## 2. Run locally

```bash
npm install
npx firebase-tools login
npm run serve
```

Open the Hosting emulator URL printed in the terminal.

## 3. Deploy manually

```bash
npm run deploy
```

Firebase Hosting will print the public shareable URL.

## 4. Push to GitHub and auto-deploy

Create a GitHub repository and push this folder to `main`. In repository Settings > Secrets and variables > Actions, add:

- `FIREBASE_PROJECT_ID`: your Firebase project id
- `FIREBASE_SERVICE_ACCOUNT`: the full JSON for a Google service account with Firebase deployment permissions

Every push to `main` runs `.github/workflows/firebase-hosting.yml`.

## Data shape

The UI accepts the recursive node shape from the supplied JSON. Existing seed nodes that only contain `id`, `name`, `generation`, and `children` remain valid. New or edited nodes can also include `gender`, `father`, `mother`, `spouse`, `location`, and `contact`.

## Architecture note

Firebase is the backend for this version. Firestore replaces a separate FastAPI/Express plus MongoDB service while still providing persistent shared storage and near-real-time sync. This keeps deployment to a single Firebase project and works cleanly with GitHub Actions.
