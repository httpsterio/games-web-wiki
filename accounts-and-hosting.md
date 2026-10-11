# Accounts and hosting

Not current work. Plans for when accounts and monetization arrive. Hosting now is Netlify, with Neon Postgres for
telemetry and feedback.

## Self hosting

Stretch goal for the future. Not now.

- Leave Netlify and Neon for a self hosted box, probably Hetzner.
- Caddy for static files.
- A small Node API.
- Postgres on the same box.
- Cloudflare in front through a tunnel, so no ports are open.
- Nightly pg_dump with restic to the owner's NAS, plus one off site copy.

## Login

- Login by emailed 6 digit code, not a link, since links open outside the installed app.
- No passwords stored.
- Codes and session tokens stored only as hashes.
- **Open.** Store emails in plain text, or only as a keyed hash that can't be read back. The hash means a leak exposes
  nothing usable, but players can never be emailed.

## Sync

- Sync per game for now.
- Settings: newest wins per key.
- Clears are merged, best time kept.
- Daily streaks are derived from results.
- Games in progress: newest wins, with a prompt.
- GDPR export and delete.

## Set aside

- Supabase and Firebase were considered and set aside.
