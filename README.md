# Vinci Scroll

Find out which cut people actually watch, before it goes live.

Vinci Scroll is a full-screen, swipe-and-react video feed for testing creative with real viewers. People watch the way they already do on their phones: swipe right on what they like, left on what they don't, react with an emoji, and every few videos leave a short written note. You get honest, in-the-moment signal on every version of your work.

It is one of Vinci's open-source tools, alongside [Vinci](https://github.com/tryvinci/vinci) and [Vinci Clips](https://github.com/tryvinci/vinci-clips).

## What it does

- **A feed people already know how to use.** Full-screen vertical video with swipe, tap-to-pause and emoji reactions, built mobile-first.
- **Feedback without a survey.** After every five videos, viewers rate content quality, engagement, relevance and technical finish, and can add a comment.
- **Every interaction recorded.** Likes, dislikes, reactions, views and written feedback are stored per video, with an analytics endpoint for each one.
- **Sign in or stay anonymous.** Google sign-in through Supabase, or a guest mode with the full feed.
- **Your videos, your storage.** Upload to your own Google Cloud Storage bucket; metadata lives in your own Supabase project.

## Good for

- Choosing between cuts, hooks or thumbnails before a campaign launches
- Testing ads and social edits with a target audience
- Collecting human feedback on generated video
- Getting a read on product demos and training videos

## Quick start

You need Node.js 18 or later, a [Supabase](https://supabase.com) project and a Google Cloud Storage bucket.

```bash
git clone https://github.com/tryvinci/vinci-scroll.git
cd vinci-scroll
npm install
cd frontend && npm install && cd ..

cp env.example .env
# fill in your Supabase and Google Cloud values

npm run dev
```

The feed opens at `http://localhost:3004` and the API runs at `http://localhost:3001`.

### Set up the database

Run `database/supabase-schema.sql` in your Supabase project's SQL editor, or use the helper:

```bash
node scripts/setup-database.js
```

### Add videos

Upload a folder of `.mp4` files to your bucket and database:

```bash
node scripts/upload-videos.js /path/to/your/videos
```

Sample clips for local testing are in `test/`.

## Configuration

Copy `env.example` to `.env` and set:

| Variable | What it is |
| --- | --- |
| `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` | Your Supabase project |
| `GOOGLE_CLOUD_PROJECT_ID`, `GOOGLE_CLOUD_STORAGE_BUCKET` | Where videos are stored |
| `GOOGLE_APPLICATION_CREDENTIALS` | Path to a service account key with access to that bucket |
| `PORT`, `FRONTEND_PORT` | API and feed ports (defaults 3001 and 3004) |
| `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `NEXT_PUBLIC_SERVER_URL` | Values the feed reads in the browser |

Keep `.env` and your service account key out of version control.

## API

| Method | Path | Purpose |
| --- | --- | --- |
| `GET` | `/api/health` | Health check |
| `GET` | `/api/videos` | The feed, paginated |
| `GET` | `/api/videos/:videoId` | One video |
| `POST` | `/api/videos/:id/view` | Record a view |
| `POST` | `/api/upload` | Upload a video (multipart) |
| `POST` | `/api/interactions` | Record a like, dislike or reaction |
| `GET` | `/api/interactions/:userId/:videoId` | A viewer's interactions with a video |
| `GET` | `/api/feedback/required` | Whether the viewer is due a feedback prompt |
| `POST` | `/api/feedback` | Submit written and rated feedback |
| `GET` | `/api/analytics/video/:videoId` | Reactions and feedback for one video |
| `POST` | `/api/auth/register`, `/api/auth/login`, `/api/auth/logout` | Accounts |
| `GET` | `/api/auth/me` | The signed-in viewer |

## How it is built

- **Feed:** Next.js 15, React, TypeScript, Tailwind CSS (`frontend/`)
- **API:** Node.js and Express (`src/`)
- **Data and sign-in:** Supabase (PostgreSQL and Auth)
- **Video storage:** Google Cloud Storage

## Roadmap

- Viewer and campaign dashboards
- Side-by-side A/B tests for two or more cuts
- Audience targeting and invite links
- Deeper analysis of written feedback

Ideas and pull requests are welcome.

## Vinci for enterprises

Vinci Scroll is part of Vinci's open-source work. Companies running creative at scale use Vinci Enterprise, which brings the Vinci system into their own cloud with a brand engine, single sign-on, approvals and a team behind it. Our creative studio, Vinci Studios, works on the same system.

[tryvinci.com](https://tryvinci.com) · [Talk to us](https://tryvinci.com/meet)

## License

[MIT](LICENSE) © Vinci
