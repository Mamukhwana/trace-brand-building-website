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
