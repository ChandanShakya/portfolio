# Reman Buddhacharya: portfolio, blog and gallery

Astro + Keystatic CMS, hosted free on Cloudflare (Workers with static assets).

## Run locally

```bash
npm install
npm run dev        # http://127.0.0.1:4321, admin at /keystatic
npm run build      # production build into dist/
```

Locally the admin edits files on disk. In production it commits to GitHub.

## Managing content (`/keystatic`)

| Section | What it controls |
|---|---|
| Blog posts | Posts with images inside the text, tags, draft toggle |
| Gallery albums | Albums and their photos (caption, location) |
| Experience / Projects | Cards in "Professional Experience" |
| Skills | Testing stack categories |
| Site Settings | Name, title, email, links, about text, resume PDF |

Tip: every save triggers a build (Workers Builds free tier: 3,000 build minutes/month). Batch your edits into one save.

## Project layout

```
src/components/   page sections and cards
src/layouts/      BaseLayout (head, SEO, nav, footer)
src/pages/        index, blog/, gallery/, 404, rss.xml
src/content/      posts (.mdoc), albums, projects, settings, skills (managed by Keystatic)
src/assets/       images uploaded through the admin (resized at build time)
src/lib/content.ts  shared content helpers
keystatic.config.ts the admin schema
```

## Deploy (one-time setup)

### 1. Create the GitHub App locally (gives you the 4 variables)
```bash
KEYSTATIC_STORAGE=github npm run dev
```
Open http://127.0.0.1:4321/keystatic, click **Create GitHub App**, and in "deployed URL" enter your final site URL
(e.g. `https://reman-portfolio.<account>.workers.dev`). Approve on GitHub, then install the app on the `portfolio` repo.
Keystatic writes `.env` with the four variables from `.env.example`. Never commit `.env`.

### 2. Push to GitHub
Push this branch and merge to `main` on `RemonBuddhacharya/portfolio`.

### 3. Create the Cloudflare project
Dashboard → Workers & Pages → Create → Import a repository (Workers Builds, free).
- Build command: `npm run build`
- Deploy command: `npx wrangler deploy`
- Project name: `reman-portfolio`
- Build variables: `NODE_VERSION` = `22`, plus the 4 Keystatic variables
- Settings → Variables and Secrets (runtime): the same 4 Keystatic values. Make the client secret and `KEYSTATIC_SECRET` Secrets.

`PUBLIC_KEYSTATIC_GITHUB_APP_SLUG` is inlined at build time, so it must be a *build* variable.

### 3b. Set the real URL
Update `site` in `astro.config.mjs` to the final URL and push.

### 4. Use it
Open `<site>/keystatic`, sign in with GitHub, write a post, save. Cloudflare rebuilds in about a minute.
Upload your CV in Site Settings → Resume.
