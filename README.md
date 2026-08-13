# ⚡ Project Manager Pro — By Waqas

A fast, GitHub-hosted project dashboard, now backed by Supabase (Postgres) instead of Google Sheets.

---

## 🗂️ Repository Structure

```
your-repo/
├── index.html                              ← Frontend (GitHub Pages)
└── .github/workflows/supabase-keepalive.yml ← Daily ping to stop the free-tier project pausing
```

---

## 🌐 How It Works

```
GitHub Pages (index.html)
        │
        │  supabase-js client (anon key, RLS-scoped)
        ▼
Supabase Postgres — table: pm_projects
```

- No backend deploy step anymore — `index.html` talks to Supabase directly via `@supabase/supabase-js`.
- Table: `pm_projects` (`id`, `category`, `name`, `link`, `created_at`). Categories are plain text on each project, exactly like the old spreadsheet's column A.
- Access is open (no login) via the Supabase **anon key**, matching the old "unlisted URL" trust model — this app is for a single private user and the link isn't shared. Row Level Security is enabled with permissive policies scoped to the `pm_projects` and `pm_keepalive` tables only; it doesn't touch your other Supabase apps in the same project.

---

## 😴 Keeping the free-tier project awake

Supabase pauses free-tier projects after 7 days without any API activity. `.github/workflows/supabase-keepalive.yml` runs daily (03:00 UTC) and sends a real read + write request against a tiny `pm_keepalive` table, which resets the inactivity timer. You can also trigger it manually from the repo's **Actions** tab (`workflow_dispatch`).

No secrets setup needed — it uses the anon key directly (safe, since RLS already limits what that key can touch).

---

## 🚀 Deploying changes

Same as before:
1. Edit `index.html` locally.
2. Push to `main`.
3. GitHub Pages picks up the change automatically (Settings → Pages → Branch: main / root).

---

## ❓ Troubleshooting

| Problem | Fix |
|---|---|
| Blank dashboard / "Supabase Sync Error" | Check the Supabase project isn't paused (Dashboard → your project) and that `SUPABASE_URL`/`SUPABASE_ANON_KEY` in `index.html` are correct |
| Project reappears as paused despite the workflow | Check the Actions tab — a workflow that stops running (e.g. repo went 60+ days without a commit) gets auto-disabled by GitHub; push any commit or re-enable it manually |
| Changes not reflecting | Hard-refresh (Supabase writes are instant, but browser may cache `index.html` — GitHub Pages CDN can take a minute to update) |
