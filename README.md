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
| `portal.html` | Parent + instructor portal. **Prototype only — mock data, no real login** | No — see warning below |
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

- **`portal.html` is a prototype.** It signs in with any email and password, and every family, note, message and invoice in it is invented. Do not put it on a public URL where a parent could find it and believe it. Either leave it out of the upload until Supabase is wired in, or rename it to something unguessable while you develop.
- Open `index.html` on a phone before you announce it. Most parents will only ever see it on a phone.
- The checkout in `index.html` is a working front-end with no payment processing behind it. Point the enrol buttons at Stripe Payment Links before you take money.

## Adding the Supabase backend later

The portal's data all lives in one place per screen, so swapping mock arrays for queries is mechanical. When you're ready, ask me for the developer handoff package — schema, row-level-security policies, and a screen-by-screen map of which portal element reads which table.

## Re-generating these files

Don't edit the files in this folder. They're compiled — a single edit in the wrong place breaks the page. Change the source design, then export again.
