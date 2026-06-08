# Retargeting Pixels — Palma Tools

## IDs

- **Meta Pixel:** `428330677033994` (Certified Inspection Services business)
- **GA4 Measurement:** `G-YBQ1SM3F7M` (Certified Inspection Services account)

> Both pixels live in the existing Certified Inspections containers. Audiences will mix between brands until you split them out — fine for now, plan to migrate to dedicated Palma containers once ad spend starts.

## Where they're wired

| Site | Status | Where |
|------|--------|-------|
| `tools.palma.llc` | ✅ wired in `index.html` head | needs redeploy |
| `permitready.vercel.app` | ✅ PR #2 — Next.js Script in `frontend/src/app/layout.tsx` | merge to deploy |
| `permit-check-mvp.vercel.app` | ✅ PR #2 — Next.js Script in `src/app/layout.tsx` | merge to deploy |
| `app.palma.llc` | ⏳ needs manual paste — see snippet below |

---

## Snippet for `app.palma.llc` (Next.js, JSX)

In the other Cowork project for `app.palma.llc`, open `app/layout.tsx` (or whichever file holds the root layout) and add this **inside the `<body>`, near the top**:

```jsx
import Script from 'next/script';

// ... inside <body> ...

{/* Google tag (gtag.js) — GA4 retargeting */}
<Script
  src="https://www.googletagmanager.com/gtag/js?id=G-YBQ1SM3F7M"
  strategy="afterInteractive"
/>
<Script id="gtag-init" strategy="afterInteractive">
  {`
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-YBQ1SM3F7M');
  `}
</Script>
{/* Meta Pixel */}
<Script id="meta-pixel" strategy="afterInteractive">
  {`
    !function(f,b,e,v,n,t,s)
    {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
    n.callMethod.apply(n,arguments):n.queue.push(arguments)};
    if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
    n.queue=[];t=b.createElement(e);t.async=!0;
    t.src=v;s=b.getElementsByTagName(e)[0];
    s.parentNode.insertBefore(t,s)}(window, document,'script',
    'https://connect.facebook.net/en_US/fbevents.js');
    fbq('init', '428330677033994');
    fbq('track', 'PageView');
  `}
</Script>
<noscript>
  <img
    height="1"
    width="1"
    style={{ display: 'none' }}
    src="https://www.facebook.com/tr?id=428330677033994&ev=PageView&noscript=1"
    alt=""
  />
</noscript>
```

Commit + push, Vercel auto-deploys.

---

## Verifying the pixels fire

After each site deploys:

**Meta:**
1. Install the [Meta Pixel Helper](https://chromewebstore.google.com/detail/meta-pixel-helper/fdgfkebogiimcoedlicjlajpkdmockpc) Chrome extension
2. Visit the site — extension icon should turn blue with `1 pixel found`, ID `428330677033994`, event `PageView`

**GA4:**
1. Open https://analytics.google.com → Realtime
2. Visit the site
3. Within ~30 sec, you should see "1 user in last 30 minutes" with the page path

If either doesn't fire: open DevTools → Network tab → filter by `fbevents` (Meta) or `gtag` (GA4) → confirm 200 responses.
