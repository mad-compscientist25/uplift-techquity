# Uplift Techquity — Cursor Migration Guide

This document provides a codebase overview and step-by-step instructions to disconnect from Lovable.dev and run the app in a local development environment.

---

## Codebase Overview

### Project Summary

**Uplift** is a React SPA that helps users discover and connect with minority-owned businesses. It focuses on economic equity through discovery and engagement.

### Tech Stack

| Layer | Technology |
|-------|------------|
| Build tool | Vite 5 |
| Language | TypeScript |
| UI framework | React 18 |
| Routing | React Router DOM v6 |
| Styling | Tailwind CSS + shadcn/ui |
| State/Data | TanStack Query (React Query) |
| Forms | React Hook Form + Zod |

### Directory Structure

```
uplift-techquity/
├── src/
│   ├── main.tsx          # App entry point
│   ├── App.tsx           # Root component, routing, providers
│   ├── index.css         # Global styles, Tailwind
│   ├── components/       # Shared components
│   │   ├── Navigation.tsx
│   │   ├── Hero.tsx
│   │   ├── FeaturesSection.tsx
│   │   ├── BusinessCard.tsx
│   │   └── ui/           # shadcn/ui primitives
│   ├── pages/            # Route-level components
│   │   ├── Index.tsx     # Landing page (/)
│   │   ├── Search.tsx    # Search businesses (/search)
│   │   ├── Auth.tsx      # Auth flow (/auth)
│   │   ├── Favorites.tsx # Saved businesses (/favorites)
│   │   ├── MessagesInbox.tsx  # Messages list (/messages)
│   │   ├── Messages.tsx  # Chat view (/messages/:businessId)
│   │   └── NotFound.tsx  # 404 fallback
│   ├── hooks/            # Custom React hooks
│   └── lib/utils.ts      # Shared utilities
├── public/               # Static assets
├── vite.config.ts        # Vite config
├── tailwind.config.ts
├── components.json       # shadcn/ui config
└── index.html
```

### Routing

| Path | Page | Description |
|------|------|-------------|
| `/` | Index | Landing (Hero, Features) |
| `/search` | Search | Business search |
| `/auth` | Auth | Authentication |
| `/favorites` | Favorites | Saved businesses |
| `/messages` | MessagesInbox | Messages list |
| `/messages/:businessId` | Messages | Single conversation |
| `*` | NotFound | 404 fallback |

### Key Dependencies

- **UI**: Radix UI primitives via shadcn, Lucide icons, Sonner toasts
- **Forms**: React Hook Form, Zod, @hookform/resolvers
- **Other**: date-fns, recharts, embla-carousel, cmdk, vaul (drawer)

### Lovable-Related Items

1. **lovable-tagger** (devDependency) — Vite plugin that adds data attributes for Lovable’s AI
2. **vite.config.ts** — Uses `componentTagger()` in development
3. **index.html** — Lovable OG image and Twitter meta
4. **README.md** — Lovable-focused docs

There are no runtime Lovable API calls or backend integrations; Lovable is only used as the development environment.

---

## Step-by-Step Migration Plan

### Step 1: Remove the Lovable Vite plugin

Edit `vite.config.ts`:

1. Remove the import: `import { componentTagger } from "lovable-tagger";`
2. Remove `componentTagger()` from the plugins array.

**Before:**
```ts
import { componentTagger } from "lovable-tagger";
// ...
plugins: [react(), mode === "development" && componentTagger()].filter(Boolean),
```

**After:**
```ts
plugins: [react()],
```

---

### Step 2: Remove lovable-tagger from dependencies

Edit `package.json` and remove the line:

```json
"lovable-tagger": "^1.1.11",
```

from `devDependencies`.

---

### Step 3: Reinstall dependencies

Run:

```sh
npm install
```

This updates `package-lock.json` and removes `lovable-tagger` from `node_modules`.

---

### Step 4: Update meta tags in index.html (optional)

Replace Lovable meta tags with your own branding:

- `og:image` — Point to your app’s social share image
- `twitter:site` — Your Twitter handle (or remove)
- `twitter:image` — Same image as `og:image`

You can keep the file as-is until you have new assets; it will not affect local development.

---

### Step 5: Update README.md (optional)

Replace the Lovable-focused content with project-specific docs. Include:

- Project name and description
- Local setup (clone, install, run)
- Available scripts
- Tech stack summary
- Deployment notes (if applicable)

---

### Step 6: Run the app locally

```sh
npm run dev
```

The dev server will start (default: `http://[::]:8080`). The app should behave the same as before, without Lovable tooling.

---

## Verification Checklist

- [ ] `vite.config.ts` no longer imports or uses `lovable-tagger`
- [ ] `lovable-tagger` removed from `package.json` devDependencies
- [ ] `npm install` runs without errors
- [ ] `npm run dev` starts the dev server
- [ ] App loads in the browser at `http://localhost:8080`
- [ ] Navigation and core routes work (Index, Search, Auth, etc.)

---

## Available Scripts

| Script | Command | Purpose |
|--------|---------|---------|
| dev | `npm run dev` | Start dev server with HMR |
| build | `npm run build` | Production build |
| build:dev | `npm run build:dev` | Build in development mode |
| preview | `npm run preview` | Serve production build locally |
| lint | `npm run lint` | Run ESLint |

---

## Notes

- **Path alias**: `@/` maps to `./src/` (configured in `vite.config.ts` and `tsconfig.json`).
- **Package managers**: The project uses npm; `bun.lockb` is present from Lovable; you can standardize on npm or migrate to bun.
- **Port**: Dev server uses port 8080; change it in `vite.config.ts` if needed.
