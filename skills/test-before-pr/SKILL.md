---
name: test-before-pr
description: Run the local dev server and verify pages before opening any PR that touches frontend code.
---

## Setup

1. Install dependencies: `npm install`
2. Start the database: `docker-compose up -d postgres`
3. Run migrations: `npx prisma migrate dev`
4. Start the dev server: `npm run dev`
5. Wait for "Ready on http://localhost:3000"

## Verify

1. Read the git diff to identify which pages changed
2. Open each affected page in the browser
3. Check for: console errors, layout issues, broken links
4. Screenshot each page at desktop (1280px) and mobile (375px) widths

## Before Opening the PR

1. Run `npm run lint` and fix any issues
2. Run `npm test` and confirm all tests pass
3. Include screenshots in the PR description