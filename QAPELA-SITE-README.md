# Qapela — GitHub Pages Upload

Upload everything in this folder (22 `.html` files + the `assets/` folder)
to the root of your `EjitechX/qapela-worker` repo. This is everything
users actually see and use — worker, business, musician, onboarding, legal
pages, the entry point (`index.html`).

**Admin pages are separate** — see the other zip.

## Before it works

1. **Paystack public key** — placeholder in `qapela-activation.html`,
   `qapela-business.html`, `qapela-checkout.html`,
   `qapela-music-marketplace.html`. Replace `pk_test_xxxx...` in all four.
2. **Resend API key** — needed for password reset emails. Set as
   `RESEND_API_KEY` in Cloudflare (see the admin/backend package).
3. **Legal page emails** — already set to `qapela.zrofeet@gmail.com` in
   `qapela-privacy-policy.html` and `qapela-terms-of-use.html`.
4. **Logo files** — `assets/qapela-logo.png`, `qapela-logo-icon.png` and
   `qapela-wordmark.png` replace the old `qapela-logo*.png`. Delete the old
   ones from the repo. The admin pages load the icon from the same `assets/`
   folder, so upload `assets/` before (or with) the admin pages.

## How deployment works

Qapela has two parts that deploy separately. Both are driven by GitHub.

| Part | What it is | Where it lives | How it goes live |
|---|---|---|---|
| **Site** | The `.html` pages + `assets/` (worker, business, admin, legal…) | Repo root of `EjitechX/qapela-worker` | **GitHub Pages** publishes it automatically on every push to `main`. Live at `ejitechx.github.io/qapela-worker/` |
| **API** | `worker.js` — signup, login, wallets, payments, emails | Same repo | **Cloudflare Workers Builds** watches the repo; every push to `main` builds and deploys it to the Worker `capella-api-5c6c` |

The pages call the API at `https://capella-api-5c6c.drthankgod08.workers.dev` (the `API_BASE` line in each page).
That URL is internal — users never see it. The Worker's name can't be renamed in Cloudflare, so it keeps this name.

**The Worker is wired to Cloudflare, not to GitHub, for its settings.** These live in the Cloudflare dashboard and survive every deploy:

- D1 database `capella-db`, bound as `DB`
- Secrets: `PAYSTACK_SECRET_KEY`, `AUTH_SECRET`, `RESEND_API_KEY`
- Optional variable: `EMAIL_FROM`
- Cron trigger: `0 * * * *`

### Updating the site (pages, logos)

1. Add/replace files in the repo root (keep the `assets/` folder).
2. Commit to `main`. GitHub Pages republishes within a minute or two.
3. Hard-refresh the browser if you still see the old version.

### Updating the API (`worker.js`)

1. Replace `worker.js` in the repo with the new file and commit to `main`.
2. Cloudflare picks up the push, builds, and deploys. Watch it under **Workers & Pages → capella-api-5c6c → Deployments**.
3. Secrets and the D1 binding are kept — you do not re-enter them.

One push can contain both site and API changes; each part deploys on its own.

### If "Latest build failed" shows in Cloudflare

Most common cause: the GitHub account or repo was renamed, which breaks the link.
Go to the Worker → **Settings → Builds**, disconnect the repository, and reconnect it as `EjitechX/qapela-worker`
(production branch `main`). Then push again or press **Retry build**.

### Things to keep in sync

- `SITE_BASE` near the top of the password-reset section in `worker.js` must match the real Pages address
  (`https://ejitechx.github.io/qapela-worker/`). It builds the reset-link and email-logo URLs. Change only that line if the username or repo name changes.
- Set `EMAIL_FROM` (e.g. `Qapela <no-reply@yourdomain>`) once your sending domain is verified on Resend.
  Until then emails go out from Resend's shared sender, which only delivers to your own Resend account email.
- Old referral codes: new accounts get `QAP-` codes. The database had no `CAP-` codes when checked (Oct 2026).
