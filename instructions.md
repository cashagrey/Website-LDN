# Local Service Lead Machine — Template Instructions

A production-ready, conversion-focused website template for local service businesses (plumbers, electricians, cleaners, locksmiths, HVAC, landscapers, etc.).

---

## 🎯 Core Mission
Turn visitors into qualified leads via:
1. Quote requests (primary KPI)
2. Click-to-call (mobile)
3. WhatsApp conversations
4. Trust-building at every scroll

---

## 🏗️ Architecture

### Pages
| Route | File | Purpose |
|---|---|---|
| `/` | `pages/Home.jsx` | Primary conversion page |
| `/service/:slug` | `pages/ServiceDetail.jsx` | SEO landing for each service |
| `/quote` | `pages/Quote.jsx` | Multi-step lead form |
| `/thank-you` | `pages/ThankYou.jsx` | Confirmation + fallback CTAs |

### Layout
- `components/layout/SiteLayout.jsx` — wraps all pages with Navbar, Footer, Sticky Mobile CTA, WhatsApp Float

### Home Sections (in order)
1. `Hero` — Command headline + live trust card + dual CTA
2. `TrustBar` — Rating / years / response time / guarantee
3. `ServicesGrid` — 6 service cards with pricing
4. `HowItWorks` — 3-step process
5. `Testimonials` — 4 verified reviews with ratings
6. `ServiceAreas` — SEO-friendly neighborhood list
7. `UrgencyCTA` — 24/7 emergency hotline block
8. `FinalCTA` — Free quote + 4 trust promises

### Quote Form
Multi-step progressive disclosure in `components/quote/QuoteForm.jsx`:
1. Service type (card grid)
2. Urgency (ASAP / This Week / Flexible)
3. Contact (name, phone, email, message)

Form submission is mocked — ready for CRM/API integration via `submit()` function.

---

## 🎨 Design System (White-Label Ready)

### One-file branding
Edit **`lib/siteConfig.js`** to change:
- Business name, phone, email, city, address, hours
- Services (title, description, benefits, pricing)
- Testimonials, service areas, trust badges
- Hero copy and CTA labels

### One-variable theming
Edit **`index.css`** `:root` block to re-theme:

```css
:root {
  --primary-action: 222 47% 11%;      /* Ink Navy — CTA color */
  --accent-success: 160 84% 30%;       /* Service Emerald — availability/trust */
  --foreground: 217 33% 17%;           /* Body text */
  --surface: 210 40% 98%;              /* Secondary background */
}
```

Preset palettes to try:
- **Plumber (default):** Navy + Emerald
- **Electrician:** `--primary-action: 38 92% 50%` (Amber) + `--accent-success: 221 83% 53%` (Electric Blue)
- **Landscaper:** `--primary-action: 142 71% 25%` (Forest) + `--accent-success: 27 80% 55%` (Terracotta)
- **Locksmith:** `--primary-action: 0 0% 9%` (Black) + `--accent-success: 0 72% 51%` (Safety Red)

### Typography
- Font: **Inter** (loaded from Google Fonts in `index.css`)
- Scale: 4rem hero / 2.5rem sections / 1.125rem body / 0.875rem labels

### Spacing Rhythm
8px/16px geometric scaling. All padding uses Tailwind's `p-5` (20px), `p-7` (28px), `p-8` (32px).

---

## ✅ Conversion Features

- ✔️ **Sticky mobile CTA bar** (Call + Quote)
- ✔️ **Floating WhatsApp** button (bottom-right, pulse animation)
- ✔️ **Click-to-call** phone links throughout
- ✔️ **Availability pulse** indicator (green dot)
- ✔️ **Multi-step form** with progress bar + validation
- ✔️ **Trust signals** above the fold (rating, reviews, license badge)
- ✔️ **Urgency block** for emergency leads
- ✔️ **Local SEO** (H1 = `[Service] in [City]`, neighborhood list)

---

## 🔌 Backend Integration (Next Steps)

Currently the form is mocked. To wire it up:

**Option A — Base44 entity (easy):**
1. Create entity `Lead.json` with fields: name, phone, email, service_type, urgency, message
2. In `QuoteForm.jsx`, replace the `submit()` mock with:
   ```js
   import { base44 } from "@/api/base44Client";
   await base44.entities.Lead.create(data);
   ```

**Option B — External CRM (HubSpot, Salesforce, Zapier):**
1. Create a backend function `functions/submitLead.js`
2. POST to the CRM endpoint from inside
3. Call `base44.functions.invoke('submitLead', data)` from the form

---

## 📱 Mobile-First Checklist

- [x] Sticky CTA bar on every page except `/quote`
- [x] Mobile hamburger menu with touch-friendly targets (48px min)
- [x] `inputmode="tel"` on phone inputs for numeric keypad
- [x] `autoComplete` attributes for autofill
- [x] Safe-area insets respected on iOS notches
- [x] Tap targets ≥ 44×44px (WCAG)

---

## ♿ Accessibility

- Semantic HTML (`<main>`, `<section>`, `<nav>`, `<footer>`)
- One `<h1>` per page
- All icon-only buttons have `aria-label`
- Form labels associated via `htmlFor`
- Focus states on all interactive elements
- Color contrast ≥ 4.5:1 (body) / 3:1 (large text)

---

## 🚀 Performance

- Inter font loaded with `display: swap`
- Hero image uses `loading="eager"` with explicit dimensions (no CLS)
- All other images should use `loading="lazy"`
- No heavy dependencies — only framer-motion for animations
- Images served from Supabase CDN (WebP when possible)

---

## 🔎 SEO Setup (Per Deployment)

When deploying for a client, also update:
1. **`index.html`** — `<title>`, `<meta name="description">`, Open Graph tags
2. **H1 on each page** — already dynamic via `siteConfig.business.city`
3. **Add LocalBusiness JSON-LD schema** to `index.html`:
   ```html
   <script type="application/ld+json">
   {
     "@context": "https://schema.org",
     "@type": "LocalBusiness",
     "name": "[BUSINESS NAME]",
     "telephone": "[PHONE]",
     "address": { "@type": "PostalAddress", "addressLocality": "[CITY]" },
     "aggregateRating": { "@type": "AggregateRating", "ratingValue": "4.9", "reviewCount": "487" }
   }
   </script>
   ```

---

## 📋 Component List

```
components/
├── layout/
│   ├── SiteLayout.jsx       # Outlet wrapper
│   ├── Navbar.jsx           # Fixed top nav with scroll glass effect
│   ├── Footer.jsx           # Dark footer with contact + services
│   ├── StickyMobileCTA.jsx  # Bottom bar on mobile
│   └── WhatsAppFloat.jsx    # Floating green WhatsApp button
├── home/
│   ├── Hero.jsx             # Headline + image + live trust card
│   ├── TrustBar.jsx         # 4-stat bar
│   ├── ServicesGrid.jsx     # Service cards grid
│   ├── HowItWorks.jsx       # 3-step process
│   ├── Testimonials.jsx     # Review cards
│   ├── ServiceAreas.jsx     # Neighborhood list
│   ├── UrgencyCTA.jsx       # Emergency hotline block
│   └── FinalCTA.jsx         # Closing CTA with promises
└── quote/
    └── QuoteForm.jsx        # 3-step lead form with validation
```

---

## 🏷️ Placeholders in Use

Everything is driven by `lib/siteConfig.js` — change it once, rebrand everywhere.

- `[BUSINESS NAME]` → `siteConfig.business.name`
- `[CITY]` → `siteConfig.business.city`
- `[PHONE NUMBER]` → `siteConfig.business.phone`
- `[SERVICES]` → `siteConfig.services[]`
- `[LOGO]` → Replace the square geometric logo in `Navbar.jsx` and `Footer.jsx`

---

## ✨ Success Criteria

- [x] Mobile-first, sticky CTAs always visible
- [x] Form reduces friction (3 steps, progress bar, smart defaults)
- [x] Trust signals within 1 viewport above the fold
- [x] White-labelable in <5 minutes (one config file + one CSS variable)
- [x] Resellable to any local service vertical
- [x] Zero backend dependencies for template demo