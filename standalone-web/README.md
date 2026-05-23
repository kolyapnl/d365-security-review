# D365 User Security Review — Standalone GitHub Page

A **zero-build, single-file** GitHub Page. No npm, no build step — commit and serve.

## How it works

`index.html` is the entire app. It loads MSAL.js from CDN at runtime and calls your
customer's Dataverse Web API using a delegated Bearer token.

## One-time setup (per GitHub repo)

### 1. Enable GitHub Pages
- Repo → **Settings → Pages → Source: Deploy from branch (main, / or /standalone-web)**

### 2. Create an Azure AD App Registration (5 min)

| Step | Action |
|------|--------|
| 1 | [Azure Portal → App registrations → New registration](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps) |
| 2 | Name: `D365 Security Review` · Supported account types: **Accounts in any organizational directory** |
| 3 | Redirect URI: **Single-page application (SPA)** · Value: your GitHub Pages URL (e.g. `https://yourorg.github.io/SecurityReview/standalone-web/index.html`) |
| 4 | After creation, copy the **Application (client) ID** |
| 5 | **API permissions → Add → APIs my org uses → Dynamics CRM → Delegated → user_impersonation** |

> The first user from each customer tenant will see a standard Microsoft consent screen — this is expected.

## Customer usage

1. Customer opens the GitHub Pages URL
2. Enters their D365 URL (`https://contoso.crm.dynamics.com`) and the Client ID
3. Clicks **Connect & Sign In** — Microsoft login popup appears
4. Dashboard loads — select a user → drill in → **Copy Review** to share with Microsoft

## What it checks (mirrors gen page)

| Area | Details |
|------|---------|
| Org overview | Active users, roles, teams, field profiles, BUs, sys admins, hierarchy config |
| Direct roles | Cross-BU, high privilege, too many roles |
| Team memberships | Access team POA risk, inherited roles, duplicates |
| Field security profiles | Too many profiles flag |
| POA & Sharing | User + team POA counts, recommendation |
| Risk rating | Low / Medium / High / Critical (same formula as gen page) |
| Review summary | Copy to clipboard or download as `.txt` |

## Sync with gen page

All Dataverse queries in `index.html` mirror the ones in `user-security-review.tsx`.  
When you update a query in the gen page, apply the same change in the corresponding
`dvFetch` / `dvCount` call in `index.html`.

## No npm, no build

The file is served as-is by GitHub Pages. There is no `package.json`.
