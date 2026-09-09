# Shelf & Seal 🫙

A small, free, self-hosted app for tracking your home canning inventory —
jar size, mouth type, canning date, storage location, and empty-jar stock —
plus a public request form for family and friends. No backend server:
everything runs as static pages on **GitHub Pages**, with your data stored
as a JSON file right in this repository.

## What's inside

| File | Purpose |
|---|---|
| `index.html` / `assets/app.js` | Your private inventory manager (add, edit, delete, view full & empty jars) |
| `request.html` / `assets/request.js` | Public page you send to family/friends, showing what's in stock and collecting requests |
| `data/jars.json` | Your inventory data, stored as a plain file in the repo |
| `assets/style.css` | Shared styling |

## 1. Put this on GitHub

1. Create a new **public** repository (it needs to be public so the request
   page can read your inventory without a login).
2. Upload all these files, keeping the folder structure.
3. In the repo, go to **Settings → Pages**, set "Deploy from a branch",
   choose your default branch and `/ (root)`, then save. GitHub will give
   you a URL like `https://yourusername.github.io/your-repo/`.

## 2. Connect the inventory app to your repo

The inventory page (`index.html`) needs permission to write to
`data/jars.json` whenever you add or edit a jar.

1. Go to **github.com/settings/personal-access-tokens/new** and create a
   **fine-grained token**:
   - Repository access: only this repository
   - Permissions: **Contents → Read and write**
   - Set an expiration you're comfortable with (you can always generate a
     new one later)
2. Open your deployed `index.html` page, click **Settings**, and enter:
   - Your GitHub username
   - The repository name
   - The branch (usually `main`)
   - The token you just created
3. Click **Save & connect**. Your inventory will load, and every add/edit/delete
   will save as a small commit to `data/jars.json`.

The token is stored only in your browser's local storage — it is never
written into any file, so it won't end up in the public repo. Anyone else
opening the page would need to enter their own token to make edits; they
can still *view* your public request page without one.

## 3. Set up the request form

The request form emails you whenever someone submits it, using the free
service [Formspree](https://formspree.io):

1. Sign up at formspree.io (free plan is plenty for personal use) and
   create a new form. Formspree gives you a form ID / endpoint like
   `https://formspree.io/f/abc12345`.
2. Open `request.html` and replace `YOUR_FORM_ID` in the `<form action=...>`
   line with your real endpoint.
3. Open `assets/request.js` and set `GITHUB_OWNER` and `GITHUB_REPO` to
   your username and repo name, so the page can show what's currently in
   stock, pulled live from `data/jars.json`.
4. Commit and push. Share the `request.html` link
   (`https://yourusername.github.io/your-repo/request.html`) with family
   and friends — no login required for them.

## How the data is structured

Each entry in `data/jars.json` represents a batch of identical jars:

```json
{
  "id": "jar-172...",
  "quantity": 6,
  "size": "Pint (16 oz)",
  "mouthType": "Regular Mouth",
  "status": "Full",
  "contents": "Dill Pickles",
  "canningDate": "2025-07-14",
  "storageLocation": "Basement Shelf A",
  "notes": ""
}
```

Empty jars use the same shape with `status: "Empty"` and blank
`contents`/`canningDate`.

## Notes & limits

- This is designed for one owner editing at a time — if you and a family
  member both edit simultaneously from different browsers, the second save
  may need a page refresh to pick up the latest version first.
- Formspree's free tier caps monthly submissions; check their pricing if
  you expect a lot of requests.
- Because the repo is public, your inventory contents (not your token) are
  visible to anyone with the link — that's what powers the live "what's in
  stock" list on the request page.
