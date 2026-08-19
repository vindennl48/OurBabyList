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

## Comparison options (add your own finds)

Every item has an "+ Add option" button under its price lines. You can add your own finds — a **name**, an optional **link**, a **price** (whole dollars), and an optional multiline **note** — to compare multiple versions of the same thing (e.g. different car seats).

- Each added entry gets a checkbox you can tick to mark it as **the one you want to buy**. Only one entry per item can be ticked at a time.
- A built-in **"Undecided"** entry appears once you've added your first find — it's selected until you pick something, so you can always switch back to "no decision yet."
- When you tick an entry, its price shows in a small badge next to the item's tag (e.g. next to "Buy new").
- Entries can be edited or deleted; the built-in Budget/Spend/Est. price lines are **not** part of this list and never change.
- Everything (the choices *and* the entries) syncs across devices in real time, same as the checkmarks.

**Firestore shape** (inside the same `checklist/emery` doc):

- `items` — `{ "<item-id>": true }` map for the "Mark purchased" checkboxes (keys are stable `data-id` slugs).
- `options` — `{ "<item-id>": { "selected": "undecided" | "<custom-id>", "custom": [ { id, name, url, price, note } ] } }`.
- `customItems` — `{ "<category-id>": [ { id, name, note, tag, price } ] }` — items you add yourself, grouped by category.

## Add your own items

Every category has an "+ Add item" button at the bottom of its list. Add your own finds — a **name**, an optional multiline **note**, an optional **tag** (Buy new / Used OK / Free), and an optional whole-dollar **price** — and they appear at the bottom of that section, synced across devices.

- The built-in items are **permanent** and never change; your added items are separate and can be **edited** or **deleted**.
- Added items get all the same features as the built-in ones: the "Mark purchased" checkbox *and* the full comparison-options list.

## Your selected total

The Money section and the hero show a live **running total** of the options you've picked, split into **Upfront** (Must-Have) and **Later** (Nice-to-Have). The Money panel breaks each down **by category**; the hero shows the two totals as highlight cards, each with a **selected** total and a **purchased** total (items you've marked "purchased"). Choosing an option adds its price; "Undecided" (or nothing) adds $0. It's computed on the fly from your selections — nothing extra is stored — and updates instantly on every device.

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
