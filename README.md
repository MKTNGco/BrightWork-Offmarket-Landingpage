# BrightWork Realty — Off-Market Listings Landing Page

**Live URL:** `https://offmarket.brightworkrealty.com`
**Repo owner:** MKTNG (GitHub organization)
**Managed by:** MKTNG.co on behalf of BrightWork Realty Advocates

---

## What This Is

A single-page lead capture landing page targeting buyers who want early access to off-market properties in Lamorinda before they hit Zillow or the MLS. The primary audience is buyers relocating from San Francisco and the broader Bay Area who are actively shopping the Moraga, Lafayette, and Orinda market.

The page collects contact information and adds leads to a private VIP list. Ben is notified and follows up with text alerts when off-market properties become available.

**Intentional design decision:** Ben Olsen and BrightWork are not prominently featured on this page. The BrightWork logo and phone number appear in the nav, but there is no agent bio, no credential section, and no advisory positioning. The page sells the concept of a private list, not the agent. This is deliberate — the audience is SF buyers who respond to access and urgency, not local brand recognition.

---

## Tech Stack

| Layer | Solution |
|---|---|
| Hosting | GitHub Pages (MKTNG org) |
| DNS / CDN | Cloudflare (managed by Side Real Estate / Luxury Presence on brightworkrealty.com) |
| Form backend | Cloudflare Worker (`bw-fub-proxy`) |
| CRM | Follow Up Boss |
| Fonts | Google Fonts — Montserrat |
| Framework | None. Plain HTML, CSS, and vanilla JS. |

---

## File Structure

```
/
├── index.html        # The entire page. All CSS and JS are inline.
└── images/
    ├── logo.png      # BrightWork logo
    └── hero.jpg      # Hero background photo
```

---

## Dependencies

### bw-fub-proxy (Cloudflare Worker)
The form submits to a Cloudflare Worker that proxies lead data to Follow Up Boss. The Worker URL is hardcoded in `index.html`:

```javascript
const FUB_PROXY_URL = 'https://bw-fub-proxy.scott-5f5.workers.dev';
const LEAD_TAG = 'off-market-lead';
const SOURCE = 'Off-Market Landing Page';
```

The Worker is in Scott@mktng.co's Cloudflare account under Workers & Pages. The FUB API key is stored as an environment variable inside the Worker — it is not in this codebase and should never be.

**CORS:** The Worker's allowed origins list includes `https://offmarket.brightworkrealty.com`. If the subdomain ever changes, the Worker code must be updated. The Worker also allows `https://buybefore.brightworkrealty.com` (the Buy Before You Sell landing page). Do not remove that entry.

### Follow Up Boss
Leads are tagged `off-market-lead` and sourced as `Off-Market Landing Page` in FUB. Form fields captured: first name, last name, email, phone. No timeline qualifier — the audience is assumed to be active buyers.

---

## Page Structure

| Section | Purpose |
|---|---|
| Nav | BrightWork logo + office phone `(925) 200-6000`. Fixed, white/blur background. |
| Hero | Full-bleed photo background, dark overlay. Badge, headline, subhead, CTA scrolls to form. |
| Trust bar | Four credibility signals: Private & Confidential, Instant Text Alerts, Exclusive VIP Access, Less Competition. |
| Split section | Left: why off-market (four benefit items). Right: lead capture form. |
| MLS notice bar | Compliance note explaining why listings cannot be shown publicly on this page. Required. Do not remove. |
| Footer | DRE# 02014153, office address, Privacy Policy link. |

### MLS Notice
The disclaimer bar at the bottom of the split section reads:

> *MLS regulations require us not to publish these listings publicly, which is why this must be a private conversation. By joining our VIP list, you gain access before anyone else — and your information is never shared with third parties.*

This is a compliance element tied to MLS rules about pre-market listing exposure. It stays on the page.

---

## Deployment

This is a static site. No build step, no dependencies to install.

1. Make changes to `index.html` or swap images in `/images/`
2. Push to the `main` branch
3. GitHub Pages serves the updated site automatically within a minute or two

GitHub Pages is configured to serve from the root of `main`. Custom domain is set to `offmarket.brightworkrealty.com` in the repo's Pages settings (Settings > Pages > Custom domain).

---

## DNS

The DNS record lives on `brightworkrealty.com`, managed by Side Real Estate / Luxury Presence.

| Type | Host | Value | TTL |
|---|---|---|---|
| CNAME | offmarket | mktngco.github.io | Automatic |

To change the subdomain or move hosting, a new DNS request must go to whoever manages `brightworkrealty.com` DNS.

---

## Brand Reference

- **Colors:** `#0bbfe0` (cyan), `#1a2f45` (navy), `#f5c800` (yellow), `#f7fafc` (off-white)
- **Font:** Montserrat (Google Fonts), weights 300 / 400 / 600 / 700 / 800
- **No em dashes** anywhere in copy. Brand constraint.
- **No mention of Side Real Estate** in any client-facing copy. Back-office relationship only.
- Brokerage DRE: `02014153`
- Office phone: `(925) 200-6000`
- Office: 455 Moraga Road, Suite 1, Moraga, CA 94556

---

## Strategic Notes

This page targets a specific buyer persona: the SF/Bay Area relocator who is actively shopping Lamorinda and wants an edge over other buyers. The urgency mechanics (blinking badge dot, "New Listings Available," "These go fast") are intentional for this audience. They are not appropriate for other BrightWork pages targeting local sellers.

If Ben develops a genuine off-market pipeline with regular inventory, this page could be expanded with a listings feed or notification system. As of launch, it functions as an intent-capture page with manual follow-up by Ben.

---

## Related Projects

| Project | Location | Notes |
|---|---|---|
| Buy Before You Sell landing page | MKTNG GitHub org | Same visual style, same FUB proxy |
| bw-fub-proxy | Cloudflare Workers (Scott@mktng.co account) | Shared by both landing pages |
| MCC real estate site | Cloudflare Workers (Scott@mktng.co account) | `moraga-country-club-realestate` worker |
| MKTNG website | Cloudflare Workers (Scott@mktng.co account) | `mktng-site-worker` |

---

*Maintained by MKTNG.co — questions to scott@mktng.co*
