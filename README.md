# PIWC Gießen Manager with Cloud Sync

This repository contains the web-based Church Manager originally
provided as `Gießen-Finance.html`.  It has been slightly
modified to support optional cloud-based backups using
[Supabase](https://supabase.com/).  The core application is
unchanged—members, contributions, expenses, attendance records,
and other data are still stored locally in the browser using
`localStorage`.  Users can export a JSON backup and import it
later.  In addition to these features, two new buttons have been
added to the header:

* **Backup in Cloud** — prompts for a Supabase email and
  password then uploads the current state to the `app_state`
  table in your Supabase project.  Subsequent backups replace
  the previous record for the same user.
* **Load from Cloud** — prompts for credentials and downloads
  the stored state from Supabase, merging it into the local
  application state.

If you choose not to use the cloud features the rest of the app
continues to work normally.  Should you wish to remove Supabase
integration entirely, delete the two additional buttons in the
header and the final `<script>` block near the bottom of
`index.html`.

## Setup

1. Create a Supabase project and enable the email/password
   provider.  Note the project URL and anonymous key from the API
   settings.
2. Create a table called `app_state` with columns:
   * `user_id` (uuid) — primary key, foreign key to
     `auth.users.id`
   * `state` (jsonb) — stores the entire application state
   * `updated_at` (timestamptz) — automatically updated on
     insert/update
3. Enable row‑level security on `app_state` and add policies
   allowing authenticated users to select, insert and update rows
   where `auth.uid() = user_id`.
4. Update the `SUPABASE_URL` and `SUPABASE_ANON_KEY` constants
   near the bottom of `index.html` with your project’s values.
5. Serve `index.html` as a static site (e.g. via Render).  Log in
   to your Supabase account when prompted to store or retrieve
   your data in the cloud.

For local development you can open `index.html` directly in a
browser.  To deploy on GitHub/Render follow the usual static
site deployment steps.