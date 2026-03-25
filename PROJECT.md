# IDX Home Search — Static Site Rebuild

## Source
- **Original:** https://idx.homeasap.com/
- **MLS Available page:** https://idx.homeasap.com/mls-available/?mlsid=flnemls
- **Platform:** WordPress + Divi Theme
- **Goal:** Single-page static recreation with URL-based state for MLS routing

## Stack
- Single `index.html` with Tailwind CSS (CDN) + vanilla JS
- Google Fonts: Lato (primary)
- HomeASAP external-checkout.js for Stripe integration

## Architecture: Single Page with Two Views

### View 1: MLS Checker (default, no `?mlsid` param)
Shows the landing hero with state/MLS dropdown selector.

### View 2: MLS Available (when `?mlsid=xxx` is in URL)
Shows the full product page with MLS name populated everywhere.

**Transition:** Selecting state + MLS + clicking "Check Availability" updates the URL to `?mlsid=xxx` via `history.pushState()` and smoothly transitions to the product view. No page reload.

**Direct links:** If someone visits `?mlsid=flnemls` directly, skip the checker, load straight into View 2.

## MLS Data Source — API (NOT hardcoded)

### State → MLS list
```
GET https://api.homeasap.com/NPlay.Services.NPlayApi/api/mls/state/{stateCode}
```
Returns array of MLS objects. Filter to `status === "Live"` only.
Response shape: `[{ id: "flnemls", name: "RealMLS (NE Florida MLS (Jacksonville))", status: "Live", ... }]`

### MLS ID → MLS details
```
GET https://api.homeasap.com/NPlay.Services.NPlayApi/api/mls/{mlsId}
```
Returns: `{ Id, Name, StateId, IsActive, ... }`
Use this when loading View 2 directly from URL to get the MLS display name.

### State dropdown values
```
AL=Alabama, AK=Alaska, AB=Alberta, AZ=Arizona, AR=Arkansas, CA=California,
CO=Colorado, CT=Connecticut, DE=Delaware, DC=District Of Columbia, FL=Florida,
GA=Georgia, HI=Hawaii, ID=Idaho, IL=Illinois, IN=Indiana, IA=Iowa, KS=Kansas,
KY=Kentucky, LA=Louisiana, ME=Maine, MD=Maryland, MA=Massachusetts, MI=Michigan,
MN=Minnesota, MS=Mississippi, MO=Missouri, MT=Montana, NE=Nebraska, NV=Nevada,
NH=New Hampshire, NJ=New Jersey, NM=New Mexico, NY=New York, NC=North Carolina,
ND=North Dakota, OH=Ohio, OK=Oklahoma, ON=Ontario, OR=Oregon, PA=Pennsylvania,
RI=Rhode Island, SC=South Carolina, SD=South Dakota, TN=Tennessee, TX=Texas,
UT=Utah, VT=Vermont, VA=Virginia, WA=Washington, WV=West Virginia, WI=Wisconsin,
WY=Wyoming
```

### Test/Prod environment detection
If URL contains "test" or "dev", use `api.testhomeasap.com` instead of `api.homeasap.com`.

## Design Tokens
- **Font:** Lato (all weights)
- **Body text:** #64748b, 18px, line-height 1.6
- **Headings:** #0f2d7a (dark navy), bold
- **Primary accent:** #1a56db (blue)
- **CTA buttons:** #42a000 green, white text, pill shape (60px radius)
- **Dark sections:** #0c2340 navy
- **Alt bg:** #e8e8e8 light gray
- **Top utility bar:** #223a49 dark, white text
- **Max content width:** 1240px
- **Promo banner bg:** matches original dark blue with urgency elements

## Page Structure

### Shared Elements (both views)
1. **Top utility bar** — phone number, Search Directory / Agent Login / Join Directory links
2. **Main header** — IDX Home Search logo (IDX-Logo-Horiz-Dark.svg), nav links, mobile hamburger
3. **Promo banner** — "Annual Spring Sale! First 200 agents take 40% off with code SPRINGIDX" + progress bar (89% claimed) + claimed count
4. **Footer** — social links (FB, Twitter, Instagram), copyright with dynamic year, Privacy Policy & Terms of Service

### VIEW 1: MLS Checker (no ?mlsid)
5. **Hero left panel** — promo banner section with sale info + progress bar
6. **Hero right panel:**
   - Phone mockup image (IDX-phones-hero.webp)
   - H1: "Capture More Leads with the #1-Ranked IDX Real Estate Home Search for Facebook Business Pages"
   - H2: "Check if your MLS is available."
   - State dropdown (hardcoded options)
   - MLS dropdown (populated via API on state change, hidden until state selected)
   - "Check Availability" button
   - Body text about brokerages
   - Brokerage logos (gray version)

### VIEW 2: MLS Available (?mlsid=xxx)
7. **MLS Confirmation banner** — green check + "[MLS Name] is available for IDX Home Search on Facebook!"
8. **Hero section:**
   - H1: "Connect with More Homebuyers & Sellers On Facebook with MLS Search"
   - Body text about becoming point of contact
   - CTA: "CLAIM YOUR OFFER"
   - Promo text: "Only ~~$149~~ $89.40 for an entire year! Use promo code SPRINGIDX at checkout."
9. **Vimeo video** — ID: 237437190 (IDX Home Search Demo)
10. **"The Entire MLS Search"** — feature section with text + phone image
11. **"Never Miss A Lead"** — feature section with images (phones + iPad)
12. **Brokerage logos bar** (dark blue version, desktop + mobile variants)
13. **Customer Reviews preview** — 3 featured reviews with star ratings + "SEE MORE REVIEWS" link
14. **"A Home Search Consumers Love"** — 4 feature cards:
    - "Be contacted from any listing"
    - "Map/Grid Views"
    - Center: phone browser mockup image
    - "High-res listing photo galleries"
    - "The most current info"
15. **"Facebook Integration & Promotion"** — pinned post feature + image
16. **"Bonus Find a Home tab"** — for qualifying pages (2000+ likes), iPad mockup
17. **"Add An Exclusive MLS Search to Your Own Website"** — website integration feature + image
18. **Testimonial block** — Randy S. quote with photo
19. **Checkout section:**
    - Left: Order Details with dynamic MLS name, checkout form (external-checkout.js)
    - Right: Company info, guarantee badge
20. **FAQ section** — 6 Q&A items
21. **Contact form** — Name, Email, Message, reCAPTCHA (or placeholder)
22. **Full Reviews section** (id="reviews") — all reviews with "Load more" functionality

## Checkout Integration
Use `external-checkout.js` same as Page Engage:
```html
<form data-ha-checkout
      data-price-id="price_IDX_annual"
      data-mode="subscription"
      data-success-url="/Directory/Services/IDX/Setup"
      data-meta-mls-id="[DYNAMIC_MLS_ID]">
  <input name="first-name" type="text" required />
  <input name="last-name" type="text" required />
  <input name="email" type="email" required />
  <input name="coupon-id" type="text" placeholder="Coupon code" />
  <button type="submit">Pay with Card</button>
</form>
```
The `data-meta-mls-id` should be dynamically set from the current `?mlsid` param.

## Images (local assets)
All downloaded to `assets/images/`:
- IDX-Logo-Horiz-Dark.svg (header logo)
- IDX-phones-hero.webp (hero phone mockup)
- IDX-phones-leads.webp (diagonal phones with notifications)
- IDX-ipad-leads.webp (iPad + phone with leads)
- IDX-phones-browser.webp (two phones with FB browser)
- IDX-facebook-post.webp (Facebook post mockup)
- IDX-ipad-fb-search.webp (iPad with FB page search)
- IDX-website-integration.png (website with WP logo)
- brokerage-logos-desktop.webp (dark blue, desktop)
- brokerage-logos-mobile.webp (dark blue, mobile)
- brokerage-logos-gray.webp (gray, for MLS checker view)
- testimonial-randy.jpg
- guarantee.png
- homeasap-logo.png
- stripe-badge.svg
- cc-icons.svg
- ssl-seal.webp
- success-check.png (green checkmark)

## Functional Elements
1. **MLS State/MLS dropdowns** — State change triggers API call, populates MLS dropdown
2. **Check Availability** — validates selection, pushes URL state, transitions to View 2
3. **View router** — on page load, check for `?mlsid` param; if present, fetch MLS name from API and show View 2
4. **Dynamic MLS name** — populates into confirmation banner, checkout order details, metadata
5. **Promo progress bar** — shows "89% claimed" with visual bar (can be static or configurable)
6. **Countdown/urgency** — claimed count + spots remaining
7. **Smooth scroll CTAs** — all "CLAIM YOUR OFFER" / "TRY IDX TODAY" buttons scroll to #checkout-container
8. **Vimeo responsive embed**
9. **Mobile hamburger menu**
10. **FAQ accordion** (expand/collapse)
11. **Reviews "Load more"** — show first 3, load more on click
12. **Contact form** — HTML form with reCAPTCHA placeholder
13. **"Change MLS" link** — in View 2, allow going back to the checker

## Content

### Pricing
- Regular: $149/year
- Sale: $89.40/year (40% off with code SPRINGIDX)

### Contact Info
- Sales: 904-549-7616, sales@homeasap.com
- Support: 904-549-7600, cs@homeasap.com
- Hours: Monday-Friday 9AM-5PM EST

### FAQ (6 items)
1. What listings does the search show? — All active MLS listings, updated every 5 minutes
2. Where does the search appear? — Facebook business page via pinned post + tab for 2000+ like pages
3. How will buyers contact me? — Contact buttons throughout + lead capture email notifications
4. Does the search also work as a standalone website? — Yes, customizable URL for external web version
5. Can subscription be used on multiple pages? — Yes, one subscription works on multiple FB pages you own/manage
6. Do I need my own listings? — Nope, featured agent for all MLS listings

### Reviews (from scraped content)
Include all reviews from the page — at least 12 reviews with ratings, dates, and reviewer names.

## Notes
- No WordPress/Divi dependencies
- No tracking scripts (GTM, Hotjar, New Relic, etc.)
- external-checkout.js handles Stripe — just wire up the form
- `data-price-id` needs real Stripe Price ID (placeholder for now)
- reCAPTCHA on contact form can be a placeholder initially
- Test mode auto-detected by URL containing "test"
