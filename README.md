# Trace Website Studio

Modern Angular 22 website and block editor with Supabase Auth, Postgres and Storage.

## Run

Use Node 24. Install with `npm ci`, then `npm start`. Configure `public/config.json` with the Supabase project URL and publishable key. Never use a service-role key in this browser application.

## Features

- Administrator login and email password recovery
- Dashboard and page library
- Add, edit, duplicate and reorder structured content sections
- Hero, text, columns, image, video, project, team, FAQ and call-to-action sections
- Media uploads and a library of original brand assets
- Live desktop/mobile preview, undo/redo and server revision history
- Separate draft save and publish operations with optimistic concurrency checking
- Page title, description, canonical URL, social image and noindex settings
- Blog post editor, categories, authors, featured images and public searchable blog
- Drag-and-drop slider layers and slide reordering, with manual position, size, fit and playback controls
- Dedicated Settings with General, SEO defaults and Integrations tabs
- Google Analytics and Search Console setup screens and read-only reporting dashboard
- Site-wide identity, contact and indexing settings

This is a custom block editor inspired by familiar page-builder workflows. It does not include the full Elementor or WPBakery feature set, plugin compatibility or arbitrary code execution.

## Database

Apply `supabase/schema.sql`, then optionally `supabase/seed.sql` to migrate the approved Trace pages. Create an Auth user, then add its UUID to `cms_members` using an owner-controlled SQL session. The application cannot grant itself administrator privileges. Do not place administrator emails, private keys or passwords in public source files.

Draft and revision tables have administrator-only read access. All writes pass through administrator-checked functions. Visitors read only published snapshots. Publishing is transactional; a stale revision cannot overwrite a newer draft. Uploaded media is public and capped at 25 MB; upload only media intended for website visitors.

## Build and test

`npm test` runs content validation and live anonymous-permission checks against the configured Supabase project. `npm run check` checks TypeScript. `npm run build -- --base-href /trace-brand-building-website/` creates the GitHub Pages build in `dist/trace/browser`.

Deploy the browser directory with `.nojekyll` to GitHub Pages. Hash routing lets admin and public page routes survive refresh on static hosting. Save and publish operations update Supabase immediately without a frontend rebuild.

## SEO deployment limit

The dashboard applies SEO tags in Angular at runtime. GitHub Pages provides no server-side rendering. Reliable social-link previews and full search optimisation require prerendering/SSR and clean URLs before a production SEO launch. Site-wide noindex remains enabled while this is a review site. No ranking guarantees are implied.

## Contact flow

The public contact form prepares an email draft; it does not send or store enquiries. A production enquiry service remains separate.

## Assets

The brand artwork, photographs and project imagery belong to their respective owners. No open-source licence for those assets is granted. Confirm copy, team roles and media rights before production launch.

## Blog and slider editing

Blog posts use the existing draft/publish and revision workflow. Their metadata lives in the page SEO JSON with `contentType: "post"`; visitors read only published snapshots. `supabase/blog-seed.sql` adds three non-destructive archive summaries with links to the full original articles. They are explicitly labelled as summaries, not full article migrations. `public/blog-seed.json` supplies the same content for local preview/import.

In the page editor, add a Slider section. Drag image/text layers in the canvas, reorder slides by their handles, or use the manual controls. Full-image fit is the default. Mobile height is separate; autoplay respects reduced-motion preferences. Draft changes must be saved and published to reach visitors.

## Google setup

Settings → Integrations accepts public identifiers only: GA4 Measurement ID, numeric GA4 Property ID, Google OAuth web client ID and Search Console property. Do not enter client secrets, service-account keys or access tokens. Enable Analytics Data API and Search Console API in the OAuth project; authorise the exact deployment origin. Configure the OAuth consent screen and Google account/property permissions in Google Cloud.

The Analytics dashboard requests read-only Google access on a deliberate Connect action. Tokens remain only in memory, expire and clear on CMS sign-out. No sample metrics are shown as real data. Live Google reporting has not been configured or tested with this project's credentials; the setup screens are ready first, as requested.

Tracking is disabled by default and loads only after an explicit visitor analytics choice when enabled with a valid Measurement ID. The footer lets visitors disable it. Search Console verification tokens added at runtime do not verify static GitHub Pages source: use DNS verification for a domain property or put the verification tag in the built HTML and redeploy.
