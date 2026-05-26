# ⚡ Project Manager Pro — By Waqas

A fast, GitHub-hosted project dashboard linked to your Google Sheets database.

---

## 🗂️ Repository Structure

```
your-repo/
├── index.html      ← Frontend (GitHub Pages)
└── Code.gs         ← Backend (paste into Google Apps Script)
```

---

## 🚀 Setup Guide (One-Time, ~10 minutes)

### Step 1 — Upload the Frontend to GitHub

1. Go to [github.com](https://github.com) → **New repository**
2. Name it anything (e.g. `project-manager-pro`)
3. Set visibility to **Public** (required for free GitHub Pages)
4. Upload `index.html` to the repo root
5. Go to **Settings → Pages → Branch: main / root → Save**
6. Your app URL will be: `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/`

---

### Step 2 — Deploy the Google Apps Script Backend

1. Open your existing Google Sheet
2. Click **Extensions → Apps Script**
3. **Delete all existing code** in `Code.gs`
4. **Paste the entire contents of `Code.gs`** from this repo
5. Click **Save** (💾)
6. Click **Deploy → New deployment**
7. Settings:
   - **Type:** Web app
   - **Execute as:** Me
   - **Who has access:** Anyone
8. Click **Deploy** → Authorize when prompted
9. **Copy the Web App URL** — it looks like:
   ```
   https://script.google.com/macros/s/AKfycb.../exec
   ```

---

### Step 3 — Connect Frontend to Backend

1. Open your GitHub Pages URL
2. You'll see a yellow **Setup banner** at the top
3. Paste your **GAS Web App URL** into the input field
4. Click **Save**
5. ✅ Your dashboard loads! The URL is saved in your browser automatically.

---

## 🔄 Updating the Backend

Whenever you change `Code.gs`:
1. Go to Apps Script → **Deploy → Manage deployments**
2. Click the **pencil (edit) icon** on your deployment
3. Change version to **"New version"**
4. Click **Deploy**

> ⚠️ You must create a new version — editing without re-deploying won't update the live API.

---

## 📋 Spreadsheet Format

Your Google Sheet (first sheet tab) must have this column order:

| A          | B            | C    |
|------------|--------------|------|
| Category   | Project Name | Link |
| Automation | My Bot       | https://... |

Row 1 is treated as a header and skipped automatically.

---

## 🌐 How It Works

```
GitHub Pages (index.html)
        │
        │  fetch() HTTP calls
        ▼
Google Apps Script Web App (Code.gs)
        │
        │  SpreadsheetApp
        ▼
Google Sheets (your data)
```

- **GET** requests → `getDashboardData` (read all projects)
- **POST** requests → add / update / delete projects & categories

---

## ❓ Troubleshooting

| Problem | Fix |
|---|---|
| "Failed to fetch" error | Check your GAS URL is correct and deployed as "Anyone" |
| Changes not reflecting | Re-deploy GAS with a **New version** |
| CORS error in browser | Make sure GAS is deployed with **Execute as: Me** and **Anyone** access |
| Blank dashboard | Check your Sheet has data in columns A, B, C starting from row 2 |
| URL forgotten | Open browser DevTools → Application → LocalStorage → `gas_api_url` |
