# MangaForge Authentication & Exclusive Owner Security

## What this module adds

- Email/password registration and sign-in through Supabase Auth.
- Email confirmation callback.
- Password reset and password update flow.
- Server-managed Supabase SSR session cookies.
- Session refresh middleware.
- Protected account page and owner-only route.
- A private, one-time owner provisioning API.
- Four public roles only: reader, creator, studio, student.
- SQL protections preventing clients from changing their role directly.
- Owner registry singleton and owner audit log tables.

## Supabase configuration

1. Create a Supabase project.
2. In Authentication → Providers, enable Email. Require email confirmation.
3. In Authentication → URL Configuration, set the Site URL to your deployed Vercel URL.
4. Add redirect URLs:
   - `http://localhost:3000/auth/callback`
   - `https://YOUR-DEPLOYMENT-DOMAIN/auth/callback`
   - `https://YOUR-DEPLOYMENT-DOMAIN/auth/update-password`
5. Run `supabase/auth_security.sql` in the Supabase SQL Editor. If starting with a clean project, first apply `supabase/schema.sql`.
6. In Vercel Environment Variables, set:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   - `NEXT_PUBLIC_SITE_URL` (your deployed URL, no trailing slash)
   - `SUPABASE_SERVICE_ROLE_KEY` (server only; never prefix with NEXT_PUBLIC_)
   - `OWNER_EMAIL` (the one email address reserved for the original owner)
   - `OWNER_SETUP_SECRET` (a randomly generated secret of at least 32 characters)

## Provision the single owner

1. Deploy the updated project with the variables above.
2. Register using the exact `OWNER_EMAIL` address through the normal `/signup` page.
3. Confirm that email through the Supabase email link.
4. Call the one-time endpoint once from a trusted terminal or API client:

```bash
curl -X POST "https://YOUR-DEPLOYMENT-DOMAIN/api/owner/provision" \
  -H "Content-Type: application/json" \
  -H "x-owner-setup-secret: YOUR_OWNER_SETUP_SECRET" \
  -d '{"email":"YOUR_OWNER_EMAIL"}'
```

5. On success, remove `OWNER_SETUP_SECRET` from Vercel and redeploy. Keep `OWNER_EMAIL` as a server-side reference if needed. Never share the setup secret.
6. Sign in normally and visit `/owner`. The server checks the owner registry; merely knowing the URL grants no access.

## Important security notes

- The service-role key bypasses RLS. It is used only in a server-only module and the one-time provisioning route.
- The setup endpoint does not accept a user ID or role from the caller. It finds the already-confirmed account matching the server-configured owner email.
- The database singleton constraint prevents a second owner row. The route refuses to provision if a row already exists.
- No public signup option or client RPC can create an owner.
- Do not enable public profile updates to the `role` column. Apply the migration as written.
- Supabase Auth handles password hashing and credential verification; the app never stores raw passwords.
- Add MFA for the owner in Supabase Auth before public launch. Configure SMTP for reliable production email delivery.
- Keep service-role and setup secrets out of GitHub, browser code, screenshots and chat.
- This module is not a substitute for a full security audit or penetration test.
