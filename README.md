# SALMAN PREMIUM STORE — repaired project

Telegram earning-task bot + FastAPI web admin panel.

## Files and layout

The GitHub repository ROOT must contain:
- `requirements.txt`
- `.python-version` and `runtime.txt`
- `Procfile` and `render.yaml`
- `app/` with `__init__.py`, `bot.py`, `config.py`, `db.py`, `main.py`

Do not set Render Root Directory to `app`.

## Render Web Service settings

- Runtime: Python 3
- Build command: `pip install -r requirements.txt`
- Start command: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
- Health check path: `/health`

Python is pinned to 3.12.10. The requirements deliberately do not pin `pydantic` or `pydantic-core` directly; Aiogram installs a compatible Pydantic build and pip can select the matching prebuilt wheel. This avoids the earlier metadata-generation failure caused by forcing a low-level Pydantic package version.

## Required Render Environment Variables

- `BOT_TOKEN` — token from Telegram BotFather
- `ADMIN_IDS` — your Telegram numeric ID (for future bot-side admin checks)
- `ADMIN_USERNAME` — web dashboard username
- `ADMIN_PASSWORD` — strong web dashboard password
- `SECRET_KEY` — long random value for sessions if/when sessions are enabled
- `DATABASE_URL` — Render PostgreSQL Internal Database URL, converted automatically to asyncpg format by the app

If using the included `render.yaml` Blueprint, Render creates the PostgreSQL database and injects `DATABASE_URL`. If creating a Web Service manually, create a PostgreSQL database separately and copy its **Internal Database URL** into the Web Service Environment.

## Deploy/update the EXISTING bot service

1. Back up the current GitHub repository first.
2. Replace the repository contents with the contents of this ZIP (the `app` folder must be at repository root, not nested inside another project folder).
3. Commit the changes to the same GitHub repository connected to the existing Render Web Service.
4. In Render, open that existing service and confirm the Build/Start commands above and the Environment Variables.
5. Trigger **Manual Deploy → Deploy latest commit**.
6. Open `/health`; it should return `{"ok":true}`. Open `/` for the admin panel and sign in with `ADMIN_USERNAME` / `ADMIN_PASSWORD`.
7. Open Telegram and send `/start` to your bot.

Do not run two Render services polling with the same Telegram bot token at the same time. Stop the old instance before starting a second one; otherwise Telegram may return a getUpdates conflict.

## Included features

Telegram: registration, referral links/bonus, profile/balance, daily bonus, task list/claim, withdrawal requests (bKash/Nagad/Rocket), force-join check, help.

Web admin: dashboard, create/activate tasks, approve/reject task claims, mark withdrawals paid or refund/reject, ban/unban users, configure bonuses/minimum withdrawal/force-join channels, broadcast.

Task completion is manually reviewed. Payouts are not automatic. Test with your own account before inviting users. Render free service local files can be ephemeral; use PostgreSQL for persistent data.
