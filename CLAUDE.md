# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start development server (http://localhost:3000)
npm run build    # Production build
npm run lint     # Run ESLint
```

## Environment

Create a `.env.local` file with:
```
NEXT_PUBLIC_SUPABASE_URL=...
NEXT_PUBLIC_SUPABASE_ANON_KEY=...
```

The Supabase client is a singleton exported from `app/utils/client.ts`.

## Architecture

**Suplatzigram** is an Instagram-like app built as a course project for learning Supabase. Stack: Next.js 16, React 19, Tailwind CSS v4, Supabase JS v2.

### Routing (App Router)

| Route | File | Description |
|---|---|---|
| `/` | `app/page.tsx` | Feed — scrollable list of posts with like toggle |
| `/post` | `app/post/page.tsx` | Create post (placeholder, not yet implemented) |
| `/rank` | `app/rank/page.tsx` | Ranking — 3-column grid sorted by likes, modal on click |

### Key conventions

- **Data source:** `app/mocks/posts.ts` exports the `Post` interface and mock data. Both `/` and `/rank` share this same array. As Supabase integration progresses, fetches replace this mock data (the `posts_new` table is queried from `app/page.tsx`).
- **Client components:** Pages that use state or effects must have `"use client"` at the top. `app/layout.tsx` is a Server Component and wraps everything with `<BottomNav />`.
- **`BottomNav`** (`app/components/BottomNav.tsx`) is a fixed bottom navigation rendered globally via the layout. It uses `usePathname()` to highlight the active tab.
- **Styling:** Tailwind v4 with CSS custom properties defined in `app/globals.css`. Use semantic tokens (`bg-card-bg`, `text-foreground`, `border-border`, `text-primary`, `text-accent`) rather than raw colors.
- **Images:** External images from `i.pravatar.cc` and `picsum.photos` are allowed via `next.config.ts`. Always use `next/image` with `fill` + a positioned parent for post images.
- **Hydration:** Avoid locale-dependent formatting without an explicit locale (e.g., use `toLocaleString('en-US')` not `toLocaleString()`). Avoid `Date.now()` or `Math.random()` in render paths.
