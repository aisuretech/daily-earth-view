# Daily Earth View - NASA EPIC Earth Images, Archive Explorer, and Space News

Daily Earth View is a Next.js web app for learners, educators, and space enthusiasts who want to explore daily NASA EPIC Earth images from the DSCOVR satellite.

Built with Next.js 16, React 19, TypeScript, Tailwind CSS, Supabase, and Vercel Cron, it combines an Earth image archive, date-based comparison tools, and curated space news in one searchable experience.

## Live Demo
- Production: https://dailyearthview.com

<!-- Use absolute production URLs for README images (https://...), not relative paths. -->
![Daily Earth View homepage preview](https://dailyearthview.com/blue-earth.png)

## Why This Project Is Discoverable on GitHub
- Targets high-intent keywords such as NASA EPIC Earth images, Earth from space, and daily satellite imagery.
- Includes practical setup and architecture details for developers using Next.js, Supabase, and scheduled ingestion.
- Covers both educational content and production implementation, helping learners and builders find relevant sections fast.
- Documents clear use cases, FAQ queries, and GitHub topics aligned to Earth observation and web development.

## Table of Contents
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Environment Variables](#environment-variables)
- [Architecture](#architecture)
- [Learning Modules](#learning-modules)
- [Deployment](#deployment)
- [SEO and Performance](#seo-and-performance)
- [Screenshots](#screenshots)
- [Use Cases](#use-cases)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [FAQ](#faq)
- [Suggested GitHub Topics](#suggested-github-topics)
- [Links](#links)
- [License](#license)

## Features

### For Learners
- Browse the latest available NASA EPIC Earth image sequence with timeline slider playback.
- Open any archive date from 2015 onward and automatically fall back to the nearest available date when needed.
- Compare two Earth dates side by side to study cloud systems and visual planetary changes.
- Read built-in learning articles about EPIC, L1, cloud behavior, climate signals, and Earth observation science.

### For Developers
- Server route ingestion pipeline fetches NASA EPIC imagery, upserts Supabase tables, and exposes date-based retrieval.
- Scheduled ingestion via Vercel Cron keeps image and space news data updated without manual intervention.
- Dynamic sitemap generation covers static pages, learning content, date routes, and news detail pages.
- End-to-end test scaffold with Playwright validates key navigation and page rendering paths.

## Tech Stack
- Frontend: Next.js 16 App Router, React 19, TypeScript, Tailwind CSS 4, rc-slider, react-datepicker
- Runtime/Platform: Node.js, Vercel hosting, Vercel Cron Jobs, Supabase PostgreSQL
- Analytics and Monitoring: Console-based ingestion logs and Vercel function logs (no dedicated analytics SDK currently configured)
- Testing: Playwright (@playwright/test)
- PWA: Not configured yet (no service worker or web app manifest in current codebase)

## Quick Start

### Prerequisites
- Node.js 20+ and npm
- Supabase project with required tables and API keys

### Install and Run
```bash
npm install
npm run dev
```

Open http://localhost:3000

### Build and Validate
```bash
npm run lint
npm run build
npm run test:e2e
```

## Project Structure
```text
dailyearthview/
  app/
    api/
      ingest/route.ts
      test-db/route.ts
    earth/[date]/page.tsx
    compare/page.tsx
    learn/page.tsx
    news/page.tsx
    news/[id]/page.tsx
    layout.tsx
    page.tsx
    sitemap.ts
  components/
    Header.tsx
    Footer.tsx
    Toast.tsx
    Breadcrumb.tsx
  utils/
    supabase.ts
    news.ts
  sql/
    setup.sql
  scripts/
    backfill-images.ts
    BACKFILL_GUIDE.md
  tests/
    basic.spec.ts
  docs/
    README.md
  next.config.ts
  vercel.json
  package.json
```

## Environment Variables
Create a .env file in the project root:

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
NASA_API_KEY=your_nasa_api_key
NEWSDATA_API_KEY=your_newsdata_api_key_optional
```

## Architecture
Daily Earth View is a content-focused Next.js platform that blends static educational pages with dynamic Earth imagery and news data pulled into Supabase.

1. A visitor opens the homepage or archive routes rendered by the Next.js App Router.
2. Client-side pages query Supabase for available dates, image sequences, and recent space news.
3. If a requested date is missing, the app can call the ingestion API to fetch NASA EPIC data for that date.
4. The ingestion route upserts `earth_daily_images`, updates `earth_available_dates`, and refreshes `earth_space_news`.
5. Vercel Cron triggers daily ingestion, while sitemap generation exposes static and dynamic routes for search engines.

```text
[Visitor Browser]
      |
      v
[Next.js App Router Pages]
      |
      +--> [Supabase Client Queries]
      |         |
      |         v
      |    [Supabase PostgreSQL]
      |
      +--> [/api/ingest]
                |
                +--> [NASA EPIC API]
                +--> [NewsData.io API]
                |
                v
          [Upsert Image + News Tables]

[Vercel Cron] --> [/api/ingest daily]
```

## Learning Modules
- EPIC satellite fundamentals and the DSCOVR viewing position near L1.
- How Earth appears from one million miles, including day-night transitions.
- Cloud pattern variability and what changing atmospheric visuals can indicate.
- Climate and Earth-observation explainers for educators, students, and curious readers.

## Deployment
Recommended deployment target: Vercel with scheduled cron execution and environment variables

```bash
npm run build
npm run start
```

Production URL:
- https://dailyearthview.com

## SEO and Performance
- Keyword-rich metadata, canonical URLs, and Open Graph/Twitter cards are set at layout and page level.
- Dynamic `sitemap.ts` includes static pages, learning paths, recent date routes, and news detail URLs.
- Image delivery uses Next.js optimization settings with modern AVIF/WebP formats for supported sources.
- Route-level content covers long-tail educational queries such as EPIC satellite, L1 point, and cloud changes.
- Next.js compression is enabled and the powered-by header is disabled for leaner production responses.
- Vercel cron-backed freshness keeps archive and news pages regularly updated for recurring crawler visits.

## Screenshots
<!-- Use absolute production URLs for all screenshot images (https://...), not ./assets paths. -->

| Feature | Preview |
|--------|--------|
| Homepage Earth image viewer | ![](https://dailyearthview.com/blue-earth.png) |
| Star forming region | ![](https://aisuretech.github.io/projects/images/spherex-frozen-co2-deposits-star-forming-region.jpg) |

## Use Cases
- Earth science teachers demonstrating daily cloud motion and planetary rotation patterns.
- Students comparing two dates to identify visible atmospheric or seasonal differences.
- Space enthusiasts exploring NASA EPIC imagery history from an easy web interface.
- Developers studying a practical Next.js plus Supabase project with scheduled data ingestion.

## Roadmap
- [ ] Add richer filtering and pagination for Earth archive exploration.
- [ ] Introduce dedicated analytics dashboards for usage and ingestion health.
- [ ] Expand E2E test coverage for archive fallback and compare workflows.
- [ ] Add optional PWA support with install prompts and offline shell behavior.

## Contributing
Contributions are welcome.

1. Fork the repository
2. Create a feature branch: git checkout -b feature/your-feature
3. Commit your changes
4. Push to your branch
5. Open a pull request

## FAQ

### Where do the Earth images come from?
Earth imagery is sourced from NASA EPIC (Earth Polychromatic Imaging Camera) on the DSCOVR mission and stored for retrieval in Supabase.

### How often is new data ingested?
A Vercel Cron job is configured to call `/api/ingest` daily, and manual date ingestion can also be triggered through query parameters.

### Can I run this project without Supabase?
Not in its current form. Core pages depend on Supabase tables for dates, image records, and space news content.

### Why do some requested dates show a different date?
NASA EPIC does not publish data for every day. The app automatically finds the nearest available date when exact data is missing.

## Suggested GitHub Topics
Add these in repository settings for discoverability:
- nextjs
- react
- typescript
- tailwindcss
- supabase
- nasa-epic
- satellite-imagery
- earth-observation

## Links
- GitHub Organization: https://github.com/aisuretech/
- Website: https://dailyearthview.com
- Contact: info@AISureTech.com

## License
Proprietary (no LICENSE file is currently committed in this repository).
