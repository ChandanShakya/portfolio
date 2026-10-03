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

1. Push this repo to GitHub (`RemonBuddhacharya/portfolio`).
2. Cloudflare dashboard → Workers & Pages → Create → Import a repository (Workers Builds, free).
   Build command `npm run build`, deploy command `npx wrangler deploy`.
   Add build variable `NODE_VERSION` = `22`.
   (The adapter outputs a Worker with static assets, so the admin's server routes run on the free Workers plan.)
3. Deploy once, then open `https://<your-site>.workers.dev/keystatic` and follow the prompt to create the GitHub App.
   Copy the values it shows into the variables listed in `.env.example` (Settings → Variables and Secrets), then redeploy.
4. Update `site` in `astro.config.mjs` to your real URL.
5. Upload your CV in Site Settings → Resume. The "Download CV" button appears automatically.
