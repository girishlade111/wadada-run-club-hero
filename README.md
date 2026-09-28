# Wadada Run Club — Hero Landing Page

A striking marketing landing page for **Wadada Run Club**, a global running community born from the spirit of Jamaica. Full-screen animated hero with liquid-glass buttons, an image slideshow, mission section with gradient-scroll typography, a timeline of runner stories, animated testimonials, a smooth-scroll hero showcase, call-to-action, footer — plus a floating AI chatbot widget wired to an n8n workflow.

Originally scaffolded with [v0.app](https://v0.app), then kept in this repo.

## Features

- **Animated hero section** — full-screen rotating image slideshow with ken-burns style transitions, sticky nav with mobile menu
- **Liquid-glass button component** — custom shadcn-style glassmorphism button
- **Text gradient scroll** — mission statement that reveals with scroll-linked gradient animation
- **Timeline** — alternating left/right runner story cards with images
- **Stagger testimonials** — staggered animated testimonial cards
- **Smooth-scroll hero** — second showcase section with scroll-driven motion
- **AI chatbot widget** (`components/chatbot.tsx`) — floating chat UI that posts to `/api/chat`
- **Chat API route** (`app/api/chat/route.ts`) — server proxy that forwards messages to an n8n webhook and returns the bot reply

## Tech stack

- **Framework:** Next.js 15.2.4 (App Router), React 19
- **Styling:** Tailwind CSS 3.4 + shadcn/ui components, `tailwindcss-animate`
- **Motion:** framer-motion
- **UI primitives:** Radix UI suite, `lucide-react` icons, `cmdk`, `vaul`, `sonner`, `embla-carousel-react`
- **Forms/data:** react-hook-form + zod, date-fns, recharts
- **Type safety:** TypeScript

## Quick start

```bash
# install dependencies
npm install        # or: pnpm install

# run the dev server
npm run dev        # -> http://localhost:3000

# production build
npm run build
npm start
```

## Environment variables

The chatbot needs a backend workflow to talk to:

| Variable | Required | Purpose |
|---|---|---|
| `N8N_WEBHOOK_URL` | Yes (for chatbot) | Webhook URL of your n8n chat workflow. Without it, `/api/chat` returns a 500 and the widget shows an error. |

Create a `.env.local` in the project root:

```bash
N8N_WEBHOOK_URL=https://your-n8n-host/webhook/your-workflow-id/chat
```

Everything else on the page (hero, timeline, testimonials) works without any env vars.

## Project structure

```
wadada-run-club-hero/
├── app/
│   ├── api/chat/route.ts   # POST proxy -> n8n webhook (needs N8N_WEBHOOK_URL)
│   ├── globals.css         # Tailwind + custom styles
│   ├── layout.tsx          # Root layout (Geist font, theme provider)
│   └── page.tsx            # Page composition: hero, mission, timeline, testimonials, CTA
├── components/
│   ├── chatbot.tsx         # Floating AI chat widget
│   ├── cta-section.tsx     # Call-to-action band
│   ├── footer.tsx          # Footer
│   ├── testimonials-section.tsx
│   ├── theme-provider.tsx  # next-themes provider
│   └── ui/                 # shadcn-style primitives (button, card, liquid-glass-button,
│                           # smooth-scroll-hero, stagger-testimonials, text-gradient-scroll, timeline…)
├── hero-section.tsx        # Full-screen slideshow hero (root-level, imported by page.tsx)
├── lib/utils.ts            # cn() class merge helper
├── public/                 # Static assets
└── styles/                 # Extra styles
```

## Deployment notes

- This is a **serverful Next.js app** — the `/api/chat` route needs a Node runtime and the `N8N_WEBHOOK_URL` secret, so it does **not** work as a static export. Deploy to a platform with server support (e.g. Vercel, Netlify with the Next.js runtime, or a VPS running `next start`).
- Images are set to `unoptimized: true` in `next.config.mjs` and most imagery is served from Vercel Blob storage URLs, so no `next/image` optimization server is required.
- ESLint and TypeScript errors are ignored during builds (`ignoreDuringBuilds`, `ignoreBuildErrors`) — intentional for v0-style iteration; tighten these if you take the project to production.

---

Built by Girish Lade — [ladestack.in](https://ladestack.in)
