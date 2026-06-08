# Cross-Link the Palma Tools (Funnel Multiplier)

Right now your three Vercel apps and the marketing site are islands. This file gives you everything you need to wire them together so a visitor who lands on any one tool discovers the rest.

---

## The Snippet (use this everywhere)

Paste this small pill anywhere in the header or footer of each Vercel app:

```html
<a href="https://tools.palma.llc"
   style="display:inline-flex;align-items:center;gap:6px;
          font-size:13px;color:#b68a4b;text-decoration:none;
          padding:6px 12px;border:1px solid rgba(182,138,75,0.3);
          border-radius:999px;">
  ← All Palma Tools
</a>
```

It uses the Palma gold (`#b68a4b`), is unobtrusive, and works in any layout. Best placement: top-right of the nav, or footer.

### React/Next.js variant (for app.palma.llc)

If the app is JSX, use this version (style as object, no quotes around the URL):

```jsx
<a
  href="https://tools.palma.llc"
  style={{
    display: 'inline-flex',
    alignItems: 'center',
    gap: '6px',
    fontSize: '13px',
    color: '#b68a4b',
    textDecoration: 'none',
    padding: '6px 12px',
    border: '1px solid rgba(182,138,75,0.3)',
    borderRadius: '999px',
  }}
>
  ← All Palma Tools
</a>
```

---

## App-by-App Instructions

### 1. `permitready.vercel.app` (Palma9099/permitready, Python)

**Where to put it:**
- Look for the base layout / template file (most likely `templates/base.html`, `templates/layout.html`, or a similar Jinja/Flask template).
- If it's Streamlit, put it in `app.py` or wherever the sidebar/header is defined — Streamlit can render raw HTML via `st.markdown(snippet, unsafe_allow_html=True)`.

**Suggested location:** top-right of the nav, or just under the page title.

### 2. `permit-check-mvp.vercel.app` (Palma9099/permit-check-mvp, TypeScript/Next.js)

**Where to put it:**
- `app/layout.tsx` (Next.js App Router) — inside the `<body>` near the top, ideally inside a header `<div>`.
- Or `pages/_app.tsx` (Pages Router) — wrap children with a header that contains the snippet.
- Or any shared `<Header />` component if one exists.

**Suggested location:** top-right of the nav.

### 3. `app.palma.llc` (built in another Cowork project — repo unknown from here)

This one I can't auto-PR because the source isn't in this project. To add the pill:

1. Open the Cowork project where `app.palma.llc` was built.
2. Find the layout/header component (Next.js: `app/layout.tsx` or a `Header` component).
3. Paste the **JSX variant** snippet above into the nav, ideally top-right.
4. Commit and push — Vercel auto-deploys.

If you'd rather, you can give me the repo name in the next session and I'll PR it the same way.

---

## palma.llc Main Nav (SitesGPT)

The marketing site needs a "Tools" link pointing to `https://tools.palma.llc`. Steps:

1. Log into https://app.oursite.co/account/login.
2. Open the palma.llc site in the dashboard.
3. Find the nav / menu editor (usually under **Settings → Navigation** or **Site Settings → Header**).
4. Add a new menu item:
   - **Label:** `Tools`
   - **URL:** `https://tools.palma.llc`
   - Open in: same tab (or new tab — your call)
5. Publish.

---

## (Optional) Cross-Recommend on Success/Empty States

When `permit-check-mvp` finishes a permit-history lookup, add a callout:
> *"Want to know if you can build a pool/addition here? Try **Permit Ready** →"*

When `permitready` finishes a feasibility report, add:
> *"Ready to start? Get a formal quote with **Palma Onboarding** →"*

These are high-intent moments — the user just finished engaging. A cross-recommendation here converts far better than banner ads.
