# Our Space

A private app where a couple runs both their shared life and their shared
business in one place — without the two bleeding into each other.

## What it is
A two-person messaging app split into separate channels — **Personal,
Business, and Faith** — so a note about an invoice never lands in the
middle of planning an anniversary. Around that sit a shared **calendar
with countdowns** (vacations, anniversaries, launches) and shared
**lists** (groceries, to-dos, honey-do). Everything is private to the
two people in the couple.

## Why it's different
Every feature is a **module** — a switch you flip in Settings: Business
channel, Faith channel, Budget, a shared assistant, a kids' calendar.
You start with what you need and turn on the rest over time. The app is
**multi-tenant** from the first row (each couple = one private tenant),
so the same build that serves us can later serve other couples with no
rebuild.

## Who it's for
- **Us, first.**
- **LDS couples** — scripture study / Come Follow Me, prayer, temple
  prep alongside the everyday coordination. The big couples apps
  (Paired, Cupla, Between) don't touch faith.
- **Copreneur couples** — spouses running a business together, who
  actually need personal and business kept apart.

The edge is distribution, not the feature list: a narrow audience we can
already reach.

## Roadmap
- **v1 (now):** messaging across 3 channels, calendar + countdowns,
  lists. Just the two of us.
- **v2:** Budget/Plaid (lifted from SOJ Finance), a deeper Faith module,
  email/push nudges (the `notify` Edge Function), and self-serve partner
  invites so other couples can sign up.
- **v3:** a shared assistant that answers from your own history
  ("what did we decide about the hotel?"), plus an onboarding
  questionnaire (faith, kids, business, distance) that decides which
  modules light up.

## Tech
- Single-file **`index.html`** — the whole app, deployed on Vercel.
- **Supabase** (Postgres + Auth + Realtime) for data. Every table is
  scoped to the caller's couple via Row Level Security.
- Design: dark navy + gold, Cormorant Garamond + DM Sans.

## Files
- `index.html` — the app Vercel deploys.
- `schema.sql` — database tables + security. Run once in Supabase.
- `seed.sql` — creates our couple + links both accounts. Run once,
  after both of us have signed up.

## Setup
1. New Supabase project → SQL Editor → paste `schema.sql` → Run.
2. Deploy `index.html` on Vercel (Framework Preset: **Other**).
3. Both partners open the URL and **sign up**.
4. Supabase SQL Editor → paste `seed.sql`, edit names/emails/date → Run.
5. Paste the project **url** and **anon key** into the `CFG` block at
   the top of `index.html`, commit.

## Architecture note (for whoever builds next)
Everything hangs off `couple.modules`. Adding Budget, Assistant, or Kids
is a new view + flipping one flag from `false` to `true` — not a
rebuild. The one genuinely new piece for going public is the self-serve
"invite your partner" flow that replaces the manual `seed.sql` step.
