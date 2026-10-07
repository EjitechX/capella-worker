# Qapela — Admin Panel

These 14 pages also go to the root of the same `EjitechX/qapela-worker`
repo (GitHub Pages doesn't separate folders — they just live alongside the
regular pages). Kept in their own zip so you can review/manage them apart
from the user-facing ones.

Only accounts listed in the `admin_roles` table can actually use these —
anyone else gets "Admin access required" from every page.

- `qapela-admin-dashboard.html` — start here, links to everything else
- `qapela-admin-pricing.html` — **do this first** after deploying: create
  at least one task type, or nothing shows up anywhere else on the platform
- `qapela-admin-users.html`, `-businesses`, `-campaigns` — oversight/search
- `qapela-admin-disputes.html`, `-verification`, `-fraud` — review queues
- `qapela-admin-pending-transfers.html` — OTP step for large withdrawals
- `qapela-admin-affiliate-conversions.html` — affiliate sales overview
- `qapela-admin-referral-payouts.html` — all referrals platform-wide
- `qapela-admin-finance.html`, `-reports` — Qapela's own revenue
- `qapela-admin-settings.html` — fees, percentages, minimums

Deployment: these pages publish with the rest of the site through GitHub Pages. Upload the `assets/` folder
first (the sidebar logo loads from it). The full deployment flow is in the main README.
