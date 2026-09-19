# Pinehurst Golf 2026

A mobile-first, single-file golf trip app for the September 23–26, 2026 Pinehurst trip. Airtable remains the source of truth for handicaps, net scores, stroke allocation, and skins.

## Preview in VS Code

1. Install the **Live Server** extension.
2. Open this folder in VS Code.
3. Right-click `index.html` and choose **Open with Live Server**.
4. On a phone, open the Live Server URL using the computer's local IP while both devices are on the same Wi-Fi.

The app asks for the dedicated Airtable personal access token on first launch. It stores the token only in that browser's local storage. Use the menu button on the home screen to clear it.

## Test base

Use a copied Airtable base by adding its base ID to the URL:

`/?base=appXXXXXXXXXXXXXX`

The app still requires a token with access to that copied base. Never test score entry against the live trip base.

## Deploy to Vercel

### CLI

```powershell
npm i -g vercel
vercel
vercel --prod
```

Choose the project folder as the root. There is no build command, framework, or output directory. The included `vercel.json` disables caching for `index.html`, so a mid-trip fix appears after a normal reload.

### GitHub

Create a repository containing `index.html`, `vercel.json`, and this README, then import it in Vercel. Select **Other** or no framework, leave build and output settings blank, and enable automatic deployments. Preview deployments are created for branches and the production URL is created from the production branch.

## Airtable token

Scott should create a dedicated personal access token limited to this base with only:

- `data.records:read`
- `data.records:write`

No schema scope is needed at runtime. Send the token privately to the group, then revoke it after the trip. Everyone holding it can read and write records in the base, so treat it as the trip password.

The app validates the token with a Courses read before saving it. Score writes are `PATCH` requests containing only one of the 18 hole-score field IDs; it never creates or deletes Airtable records. Offline score writes are queued in local storage and retried when the connection returns.

## Notes

The live app polls the current Airtable data every 15 seconds while visible and refreshes on focus. URL `base` overrides the production base ID for safe testing. Airtable field IDs are kept inline so field renames do not change the app's behavior.
