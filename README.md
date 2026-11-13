# SALMAN PREMIUM STORE — Telegram earning-task bot starter

Python 3.11+, Aiogram 3, FastAPI admin dashboard, SQLAlchemy async database.

## Included
- Telegram registration and referral deep links
- Balance/profile, daily bonus, task listing and claim review
- Withdrawal requests for bKash/Nagad/Rocket with minimum amount
- Web admin: dashboard, tasks, task review, withdrawals, users ban/unban, settings, broadcast
- PostgreSQL on Render (SQLite fallback for local testing)

## Important limitations before real launch
This is a starter implementation, not a verified copy of another bot. Task completion is manually reviewed by an admin. Channel membership checking works only when channel usernames are configured and the bot can access them. Website visits, app installs, videos, shortlinks and payment payouts are **not automatically verified** in this starter. Add trusted verification providers and anti-fraud controls before paying users. Never promise earnings or accept deposits from users.

## Local run
1. Create a bot with Telegram's official BotFather and copy the token.
2. Copy `.env.example` to `.env`, fill `BOT_TOKEN`, `ADMIN_IDS`, `ADMIN_USERNAME`, `ADMIN_PASSWORD`.
3. Install Python 3.11+ and dependencies:
   `pip install -r requirements.txt`
4. Export the environment variables from `.env` (or use a dotenv runner).
5. Start: `uvicorn app.main:app --reload`
6. Open `http://127.0.0.1:8000/` and sign in using `ADMIN_USERNAME` / `ADMIN_PASSWORD`.

## Deploy on Render
1. Push this project to a GitHub repository.
2. In Render choose **New + → Blueprint**, connect the repo and apply `render.yaml`.
3. Set `BOT_TOKEN`, `ADMIN_IDS`, and a strong `ADMIN_PASSWORD` in Render Environment.
4. Deploy. Open the service URL; the browser asks for admin username/password.
5. Make the bot an administrator in each force-join channel. In Admin → Settings, set `force_join_channels` to comma-separated usernames, e.g. `@channel_one,@channel_two`.
6. In BotFather set a description and commands if desired.

## Security checklist
- Use a long unique admin password; never commit `.env` or tokens to GitHub.
- Keep the Render database private; take backups before migrations.
- Put the dashboard behind HTTPS (Render provides HTTPS).
- Restrict admin routes further with an IP allowlist or stronger login/2FA before handling meaningful funds.
- Add rate limits, audit logs, idempotent payment reconciliation and fraud review before production.
- Test with small internal accounts first. Admin password is HTTP Basic Auth; use only over HTTPS.

## Commands
- `/start`
- `/withdraw amount method account` (example: `/withdraw 100 bkash 01XXXXXXXXX`)
- `/help`

## Admin
- `/` dashboard
- `/tasks` create and activate/deactivate tasks
- `/claims` approve/reject submissions
- `/withdrawals` mark paid or reject/refund
- `/users` ban/unban users
- `/settings` edit bonus, referral, minimum withdrawal, force-join channels
- `/broadcast` send a message to active users

## Next engineering steps
Add pagination/search, admin audit trail, transaction history UI, automatic channel-task verification, task proof upload, duplicate-account detection, rate limiting, proper CSRF protection, and a payout integration with reconciliation. Validate legal/payment-provider requirements in your country before accepting users.
