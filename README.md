# Our Baby List — for Emery Laine ♡ due February 3, 2027

A single-page checklist of everything to buy before our baby girl comes home, with budget + "spend here" options, honest prices, and safety rules.

**Design:** Blush & Petal (light, soft, warm) — baby-girl pinks, purples, magentas, maroons.
**Backend:** Firebase Firestore — checkmarks sync across devices in real time.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The checklist page (content + Firebase sync) |
| `styles.css` | All styling for the page |
| `firebase.json` | Firestore deploy config (rules + indexes) |
| `firestore.rules` | Firestore security rules (open: `allow read, write: if true`) |
| `firestore.indexes.json` | Firestore indexes (empty) |
| `AGENTS.md` | Conventions for AI coding agents |

## Checkmarks sync across devices

Checking a box saves it to Firestore (project `ourbabylist`, collection `checklist`, document `emery`). Every device viewing the page loads the same checkmarks and updates within a second of any change — no account or login needed.

**Security note:** the rules are intentionally **open** (`allow read, write: if true`). Anyone who finds the project ID can read or write the checklist. That's fine for a non-sensitive personal list; tighten `firestore.rules` if that changes.

To update the rules, edit `firestore.rules` and redeploy:

```bash
firebase deploy --only firestore --project ourbabylist
```

## Publish on GitHub Pages

1. Create a repository on GitHub (e.g. `our-baby-list`).
2. Push this folder to the repo:
   ```bash
   cd /Users/mitch/Documents/code/OurBabyList
   git init
   git add .
   git commit -m "Our Baby List"
   git branch -M main
   git remote add origin https://github.com/<your-username>/our-baby-list.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Source = "Deploy from a branch" → Branch = `main` / `(root)` → Save**.
4. Live at `https://<your-username>.github.io/our-baby-list/`.

## Edit the content

Everything is plain HTML in `index.html`. Change a price, name, or note by editing the text inside the `.item` blocks. Checkmarks save to Firestore automatically — no other wiring needed.

Prices shown are estimates and include the insurance-covered breast pump ($0) and free formula samples where noted.
