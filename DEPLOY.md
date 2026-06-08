# Deploy `tools.palma.llc` — Step-by-Step

This folder contains a ready-to-deploy static landing page. Pick a path below. After deploying, complete the **Post-Deploy Checklist** at the bottom.

---

## PATH A — Fastest (Drag-and-Drop to Vercel, ~3 minutes)

1. Go to **https://vercel.com/new** and sign in (use the same account that hosts `permitready.vercel.app` and `permit-check-mvp.vercel.app`).
2. Click **Add New → Project**, then drag the entire `tools-landing` folder into the upload area. (If drag-and-drop isn't visible, use the CLI: `npx vercel --prod` from inside the folder.)
3. Project name: `palma-tools` (gives you `palma-tools.vercel.app`).
4. Click **Deploy**. You'll have a live URL in ~30 seconds.

### Attach `tools.palma.llc`

In the Vercel project dashboard:

1. **Settings → Domains** → enter `tools.palma.llc` → **Add**.
2. Vercel shows you a CNAME record to add.

### Add the DNS record in Squarespace

1. **Squarespace → Home → Settings → Domains → palma.llc → DNS Settings**.
2. Under **Custom Records**, click **Add Record**:
   - **Type:** `CNAME`
   - **Host:** `tools`
   - **Data:** `cname.vercel-dns.com`
3. Save. Propagation takes 5–30 minutes; SSL auto-provisions.

---

## PATH B — Git-Based (Cleaner long-term)

1. `cd tools-landing && git init && git add . && git commit -m "Initial tools landing page"`
2. Push to a new GitHub repo (`palma-tools-landing`).
3. Vercel → Import Git Repository → Deploy.
4. Follow the subdomain + DNS steps above.

---

## POST-DEPLOY CHECKLIST

These three steps unlock the full value of the page. Do them right after your first deploy.

### 1. Enable Vercel Analytics (30 seconds, free)

In the Vercel project dashboard:
1. Click **Analytics** in the left sidebar.
2. Click **Enable** (it's free on the Hobby plan up to 2,500 events/month).

Done. The page already includes the Vercel Analytics script (`/_vercel/insights/script.js`). Once enabled in the dashboard, you'll see pageview stats + click events within 24 hours. Each tool card has a `data-track` attribute, so you'll also see which tool gets clicked most: `card-permit-ready`, `card-onboarding`, `card-permit-history`, `cta-talk`.

### 2. Set up Microsoft Clarity (3 minutes, free, session recordings)

1. Go to **https://clarity.microsoft.com/** → sign in with Microsoft/Google/email.
2. Click **Create new project**:
   - Name: `Palma Tools`
   - Website URL: `https://tools.palma.llc`
3. After creation, you'll see a **Project ID** (looks like `abc123xyz`).
4. Open `index.html` in this folder.
5. Find this line near the bottom:
   ```js
   })(window, document, "clarity", "script", "YOUR_CLARITY_PROJECT_ID");
   ```
6. Replace `YOUR_CLARITY_PROJECT_ID` with the ID from step 3.
7. Re-deploy (drag folder back to Vercel, or `git push`).

Within a few hours, Clarity will show you live heatmaps and a video recording of every visitor session. Use this to see where people hesitate, rage-click, or scroll past.

### 3. Cross-link your other tools (the funnel multiplier)

Right now your tools are islands. Turn them into a funnel with these three edits:

#### A. Add "Tools" link in palma.llc main nav (Squarespace, 2 min)
1. Squarespace editor → edit site navigation.
2. Add a new link:
   - **Label:** `Tools`
   - **URL:** `https://tools.palma.llc` (mark as "Open in new tab" or inline — your preference)
3. Publish.

#### B. Add "More Tools" badge to each Vercel tool (5 min each)

For each of `permitready.vercel.app`, `app.palma.llc`, and `permit-check-mvp.vercel.app`, add a small link back in the header or footer of the app. Simplest HTML snippet to paste into each tool's layout:

```html
<a href="https://tools.palma.llc"
   style="display:inline-flex;align-items:center;gap:6px;
          font-size:13px;color:#b68a4b;text-decoration:none;
          padding:6px 12px;border:1px solid rgba(182,138,75,0.3);
          border-radius:999px;">
  ← All Palma Tools
</a>
```

Place it near the top-right of the nav, or in the footer. Visitors who land on one tool now know two more exist.

#### C. (Optional) Cross-recommend on success/empty states

When `permit-check-mvp` finishes a permit-history lookup, add a callout:
> *"Want to know if you can build a pool/addition here? Try **Permit Ready** →"*

When `permitready` finishes a feasibility report, add:
> *"Ready to start? Get a formal quote with **Palma Onboarding** →"*

These are high-intent moments — the user just finished engaging. A cross-recommendation here converts far better than banner ads.

---

## Files in this folder

| File | Purpose |
|------|---------|
| `index.html` | The landing page. Single file, no build step. |
| `vercel.json` | Deployment config: clean URLs + security headers. |
| `assets/logo-128.webp` | Nav/footer logo — 2.5 KB (was 1,420 KB). |
| `assets/logo-256.webp` | Retina logo — 5.6 KB. |
| `assets/og-image.jpg` | Custom social-share image — 65 KB. |
| `assets/favicon-32.png` | Browser tab favicon. |
| `assets/apple-touch-icon.png` | iOS home-screen icon. |
| `DEPLOY.md` | This file. |

## Editing later

- **Copy changes:** open `index.html` and search for the English text, or edit the `I18N.en` and `I18N.es` dictionary objects at the bottom of the file.
- **Brand colors:** CSS variables at the top of the `<style>` block (`--gold`, `--charcoal`, etc.).
- **Tool URLs:** search for `https://` in the three `<article class="tool-card">` blocks.
- **Re-deploy:** drag the folder to Vercel again, or `git push`.
