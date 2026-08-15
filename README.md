# Sprout Code — deployment files

Every file here is self-contained. No build step, no npm, no framework.
Upload the folder contents as-is.

## What each file is

| File | What it is | Public? |
| --- | --- | --- |
| `index.html` | The Sprout Code marketing site — courses, schedule, checkout, syllabus modals | Yes |
| `curriculum.html` | Curriculum handbook: three tracks, the ladder, week-by-week scope | Yes |
| `syllabus.html` | Parent syllabus, one page per course. Printable | Yes |
| `worksheets.html` | 100 printable student worksheets | Link from the portal, not the public site |
| `portal.html` | Parent + instructor portal. Real Supabase sign-in once configured; screen content still mock | Not until configured — see below |
| `robots.txt` | Keeps the worksheets and portal out of search results | Yes |
| `_headers` | Cloudflare Pages security headers | Yes |

## Deploy — GitHub to Cloudflare Pages

1. Create a new GitHub repo, e.g. `sprout-code-site`. Public or private, both work.
2. Upload the contents of this `deploy/` folder to the repo root — so `index.html` sits at the top level, not inside a folder.
3. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git** → pick the repo.
4. Build settings: leave **Framework preset** as *None*, leave **Build command** empty, set **Build output directory** to `/`. Save and deploy.
5. Wait about a minute. You get a `something.pages.dev` URL. Open it and confirm the site loads.
6. Pages → your project → **Custom domains** → **Set up a domain** → enter your domain.
   - If your domain's nameservers are already on Cloudflare, it configures itself.
   - If not, Cloudflare shows you a CNAME record. Add it at your registrar. Propagation is usually minutes, occasionally a few hours.
7. HTTPS is issued automatically. Nothing to buy or install.

From then on, any push to the repo redeploys the site.

## Before you point the domain at it

- **`portal.html` needs your Supabase keys before it means anything.** Until they're in, it runs in demo mode: any email and password signs you in, and every family, note, message and invoice is invented. A banner on the login card says so. Don't put it on a public URL in that state — either leave it out of the upload for now, or give it an unguessable filename while you develop.
- **To connect it**, open `portal.html` in a text editor and find this block near the top:

  ```js
  window.SPROUT_CONFIG = { url: "https://YOUR-REF.supabase.co", anonKey: "YOUR-ANON-KEY" }
  ```

  Replace the two placeholder strings with the values from Supabase → Project
  Settings → API. A plain find-and-replace of `YOUR-REF` and `YOUR-ANON-KEY` is
  safe — those are the only two edits this compiled file tolerates. Or send me the
  two values and I'll bake them in and hand you a fresh export.

  The anon key belongs in the browser; that's what it's for. Row-level security is
  what protects the data. Never put the `service_role` key in this file.
- Open `index.html` on a phone before you announce it. Most parents will only ever see it on a phone.
- The checkout in `index.html` is a working front-end with no payment processing behind it. Point the enrol buttons at Stripe Payment Links before you take money.

## Adding the Supabase backend later

Sign-in is done. The remaining work is swapping each screen's mock array for a
query, which is mechanical — the `supabase/` folder has the schema, the
row-level-security policies, the seed data, the email function and a
screen-by-screen map of which portal element reads which table.

## Re-generating these files

Don't edit the files in this folder. They're compiled — a single edit in the wrong place breaks the page. Change the source design, then export again.
