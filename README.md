# Von Visual Designs — Client Progress Portal

This starter contains:
- `index.html` — client portal with email one-time-code login and project progress
- `admin.html` — designer portal to add, edit, and delete projects
- `schema.sql` — database setup and row-level security policies

## 1. Create the Supabase backend
1. Create a project at https://supabase.com/.
2. Open **SQL Editor**, paste all of `schema.sql`, and run it.
3. In **Project Settings → API** (or **Connect**, depending on the dashboard), copy the Project URL and the publishable/anon public key.
4. The supplied Project URL and publishable key have already been filled into `index.html` and `admin.html`. If you rotate either value, update both files. The public anon/publishable key is intended for browser use; **never put a service-role key in either HTML file**.
5. In **Authentication → URL Configuration**, add the deployed client and admin website URLs to allowed redirect/site URLs as needed.
6. In **Authentication → Email Templates**, set the Magic Link / OTP email template to display the one-time token, using `{{ .Token }}`. The user enters this code on the page.
7. Check Auth email rate limits and SMTP configuration in Supabase. For production, configure a trusted SMTP provider so messages reliably reach customers.

## 2. Authorize the admin
The SQL currently authorizes only `isaac@vonvisualdesigns.com` as the admin. If you use a different admin email, change it in the `public.is_von_admin()` SQL function and run the updated function definition. The admin must verify the emailed code before they can manage projects.

## 3. Publish
Upload `index.html` to your client portal site and `admin.html` to your admin site (for example, GitHub Pages). Keep the files separate if you want separate URLs, such as:
- `clients.vonvisualdesigns.com` → `index.html`
- `admin.vonvisualdesigns.com` → `admin.html`

## 4. Add a client
1. Sign in to the admin portal with the authorized email.
2. Enter the client's email exactly as they will use it to sign in.
3. Add the project name, progress percent, stage, notes, and designer contact.
4. Save. The client can now sign in to the client portal with that same email.

## Important security notes
- These pages use Supabase Auth OTP plus database row-level security; the browser anon key is not a secret.
- Never expose a Supabase service-role key in frontend code.
- Clients can only read projects assigned to their verified email. Only the admin email defined in SQL can create/update/delete projects.
- A project email is not proof that the customer owns that address; Supabase verifies access by sending the login code.
- Before launch, test with a separate client email and verify that one client cannot see another client's project.
