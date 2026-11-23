# MangaForge — starter project

**Create. Publish. Discover. Animate.**

This repository contains the first runnable MangaForge web foundation: a responsive dark visual system, original hero artwork, home page, starter routes, Supabase client wiring, a basic browser sketchpad, and an initial database schema.

## Important delivery status

This is **Milestone 1 / runnable foundation**, not the complete production ecosystem described in the master brief. It does not yet include a production owner provisioning process, complete RLS model, private messaging, full manga editor, publishing pipeline, moderation workflows, finished learning PDFs, email delivery, or end-to-end test coverage. The `/admin` route is explicitly non-functional and grants no access. Sample manga catalogue cards are illustrative starter content.

Do not launch this as a public production platform until the remaining modules are implemented and tested.

## Run locally

1. Install Node.js LTS.
2. Install dependencies with `npm install`.
3. Copy `.env.example` to `.env.local`.
4. Add your Supabase project URL and anon key when the Supabase project is ready. The public pages and drawing sketchpad can run without Supabase.
5. Run `npm run dev` and open `http://localhost:3000`.

## Deploy to GitHub and Vercel

1. Create a GitHub repository named `mangaforge`.
2. Upload the contents of this folder to the repository root (not the containing ZIP folder).
3. In Vercel, choose **Add New → Project**, import the repository and keep the detected Next.js settings.
4. Add `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` in Vercel project environment variables after creating a Supabase project.
5. Deploy.

## Supabase setup

- Create a Supabase project.
- Configure email authentication and your allowed redirect URLs.
- Review and apply `supabase/schema.sql` in a development project.
- Create private storage buckets and policies in a later storage milestone.
- Never put `SUPABASE_SERVICE_ROLE_KEY` in a `NEXT_PUBLIC_` variable or client-side code.

## Included routes

- `/` home
- `/explore` catalogue starter
- `/create` basic sketchpad
- `/reader` reader layout starter
- `/creator` creator dashboard starter
- `/studios` studio ecosystem introduction
- `/learn` learning hub outline
- `/help` help topics
- `/about`, `/terms`, `/privacy`
- `/signin`, `/signup` Supabase Auth wiring
- `/admin` non-functional placeholder only

## Suggested next milestones

1. Supabase schema, secure profiles, role onboarding and account lifecycle.
2. Server-verified single-owner provisioning, admin authorization and audit events.
3. Manga projects, chapters, private drafts, validated storage and publishing.
4. Full multi-page editor with layers, panels, bubbles, tones, autosave and export.
5. Reader history, bookmarks, follows and creator notifications.
6. Studio opportunities, applications, collaboration permissions and messaging.
7. Complete learning content, five substantial PDFs and progress tracking.
8. Reporting, moderation, appeals, feedback and email notifications.
9. Automated tests, security review, accessibility, backups and production deployment.

## Artwork

`public/images/mangaforge-hero.png` is original generated concept artwork created for this project. Review it and replace it with commissioned/approved final artwork if required.


## Authentication module
See `README-AUTH.md` for the email/password authentication, password recovery, secure session setup, and one-time exclusive owner provisioning instructions. Apply `supabase/auth_security.sql` after `supabase/schema.sql`.
