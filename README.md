# LPC & OPC Study Companion — Supabase picture-storage update

This GitHub Pages update keeps the existing local study-data key (`lpcopc-study-companion-v2`) so the app can continue loading the topics, slides, notes and cards already saved in the same browser. It moves slide pictures out of browser Local Storage and into a **private Supabase Storage bucket**.

## What changes

- New slide pictures are resized/compressed in the browser and uploaded directly to Supabase.
- Slide HTML stores a small Supabase object path instead of a Base64 image, reducing browser Local Storage usage.
- Existing embedded slide pictures can be migrated in one operation from the Cloud Storage panel (cloud icon on the home screen).
- Migration uploads each picture first, then replaces its local Base64 data with a cloud reference. If an upload fails, that picture remains embedded locally and can be retried.
- The app creates short-lived signed URLs to display pictures from the private bucket.
- Study text, notes, flashcards and topic order remain local to this browser. This update does not sync all study data between devices.
- The app uses Supabase Auth email/password accounts and Row Level Security. Do not put a Supabase `service_role` or secret key in the app.

## Supabase setup (free plan)

1. Create a project at <https://supabase.com/dashboard>.
2. In the project, open **SQL Editor**, create a new query, paste the contents of `supabase-setup.sql`, and run it. This creates the private `study-images` bucket and per-user storage policies.
3. Open **Project Settings → API** (or the API Keys page) and copy:
   - Project URL, for example `https://your-project-ref.supabase.co`
   - The **publishable** key (or legacy `anon` public key)
4. Never use or expose the `service_role` / secret key.
5. In **Authentication → URL Configuration**, add your published GitHub Pages app URL to the allowed Redirect URLs. For example: `https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/`.
6. Keep email confirmation enabled if you prefer verified accounts. When you create an account in the app, check your email, confirm it, return to the app and sign in. If you disable confirmation for a personal-only project, use a strong password and keep the project credentials private.

## Safe migration of existing pictures

**Do this on the same phone, browser and GitHub Pages app installation where your existing pictures are currently saved. Do not clear Safari website data, remove the Home Screen app, or switch browsers before migration.**

1. First deploy this update to GitHub Pages by replacing the project files with the files in this ZIP.
2. Open the updated app in the same browser that contains your current study data.
3. On the home screen, tap the cloud icon.
4. Tap **Download study backup** and keep the downloaded JSON file somewhere safe. This backup includes your current local topics and any embedded image data.
5. Enter the Supabase Project URL and publishable/anon key, then save the connection settings.
6. Create an account, confirm your email if requested, and sign in.
7. Tap **Migrate existing pictures**. Keep the app open and connected to the internet until it finishes. The progress indicator shows progress. Failed items remain in the local slide data and can be retried.
8. Open several slides and confirm their pictures display before deleting or clearing anything from the old browser storage. Do not clear browser data as part of this migration.

The app's migration is restartable: already migrated images are stored as paths, and only remaining Base64 images are selected for another migration run.

## Publishing

Replace these files in the root of the GitHub repository:

- `index.html`
- `manifest.webmanifest`
- `sw.js`
- `README.md`
- `supabase-setup.sql`

Commit the changes and wait for GitHub Pages to redeploy. The service-worker cache version is bumped so the new app code can load. Supabase requests are excluded from the service-worker cache.

## Free-plan limits and availability

Supabase currently lists 1 GB Storage, 5 GB egress, and 5 GB cached egress on its Free plan. Free projects may be paused after one week of inactivity. This is not unlimited storage; monitor usage in the Supabase dashboard. Check current limits before relying on it for long-term use: <https://supabase.com/pricing>.

## Important data boundaries

- Pictures in the private bucket are cloud-hosted and can be accessed only by the signed-in account's storage policies or a time-limited signed URL.
- The local slides, notes, review cards and topic order remain in browser storage. They are not automatically synced to another device.
- Keep the downloaded backup. Supabase Free does not include automatic database backups for this local study data, and this app does not upload the topic database to Supabase.
- If you clear browser data, your local study text and notes may be lost even though migrated pictures remain in Supabase.

## Topic completion and mixed-topic tests

- Pressing **Finish** on the final study slide returns directly to the main topic menu.
- The main menu includes **Take a test**.
- Select one topic, multiple topics, or all topics. Each topic shows how many saved Review cards it contains.
- Choose a question count or select **Use all available cards from selected topics**.
- The app shuffles the selected saved Review cards across topics before starting; it does not follow topic or card order.
- Reveal each answer, then mark **I got it right** or **I got it wrong**. The final screen shows your percentage, total questions, correct answers and incorrect answers.
- Tests use the cards already saved in each topic's Review section, including manually edited or added cards. Test results are session-only and are not added to study data.
