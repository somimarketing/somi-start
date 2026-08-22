# start.somi.city — deploy

Static site. One HTML file, everything inlined. No build step.

## Option A — new Vercel project (2 min)

1. vercel.com → Add New → Project → **Deploy without Git** (or drag this folder in)
2. Framework preset: **Other**. Build command: none. Output dir: `./`
3. Project name: `somi-start`
4. After it deploys: Settings → Domains → add `start.somi.city`
   - `*.somi.city` already has a wildcard A record from the somi-crm setup,
     so it verifies instantly. No registrar work.

## Option B — inside the existing somi-crm project (better)

Drop `index.html`, `og.png`, `favicon.png` into `public/start/` and push.
Then add the host rewrite in `next.config.ts`, same pattern as `wa.somi.city`:

```ts
async rewrites() {
  return [
    {
      source: '/:path*',
      has: [{ type: 'host', value: 'start.somi.city' }],
      destination: '/start/:path*',
    },
  ]
}
```

Add `start.somi.city` under Vercel → somi-crm → Domains.

Why this is better: same project, same analytics, and the WhatsApp
click logging in `public.link_clicks` already works for the CTA.

## Files

- `index.html` — the page (126 KB, self-contained)
- `og.png` — social preview, 1200x630
- `favicon.png` — 256x256
- `vercel.json` — cache + security headers

## After it is live

- Test the WhatsApp CTA: it points at `wa.somi.city/hola`
- Consider `wa.somi.city/start` with its own message so you can
  separate Somi Start leads from everything else in `link_clicks`
