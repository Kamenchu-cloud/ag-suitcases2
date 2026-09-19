# AG Suitcases Collection — Deployment Notes

This folder is the deployable site. Push it to the `ag-suitcases` repo (replace
the existing `index.html` and `images/`) and GitHub Pages will pick it up —
no build step, no dependencies.

## What changed from the previous version

**Catalogue (Gate 1)**
- Went from 14 loosely-labelled "collections" to **15 distinct products**,
  each with a real config (piece count), material description, and a
  catalogue reference number for traceability.
- Two groups that mixed genuinely different shell designs under one label
  (old groups 06 and 12) were **split into separate products** so a shopper
  isn't shown one "product" that's actually two different items.
- One group (old group 11) was **dropped as a standalone product** — its
  photos were WhatsApp screenshots with chat UI visible, not usable product
  imagery. Its colours are mentioned as a text note on the closest match
  (Executive Frame Set) instead of being faked with images.
- See `CATALOGUE-MAPPING.csv` for the full product → image → variant map.

**Images (Gate 2)**
- The site was serving 100–178px thumbnails in most places even though
  full-resolution originals existed alongside them. Every product photo on
  the live site now comes from the **original high-resolution file**
  (typically 960px–1600px), resized to two clean web sizes:
  `images/lg/` (product page + zoom, capped at 1200px) and `images/sm/`
  (cards, thumbnails, cart, capped at 560px).
- Each image was reviewed individually (contact sheets, not spot checks) to
  confirm framing, then deliberately mapped to a specific colour/variant —
  nothing was auto-substituted.

**Product page (Gate 3)**
- Real colour/style swatches: clicking one swaps the main image, the
  gallery highlight, and the pre-filled WhatsApp message — this is genuine
  variant selection, not decorative text.
- Tap-to-zoom lightbox on the main image.
- Specs block, delivery/pickup/returns note, and a sticky mobile "Add to
  Cart / WhatsApp" bar on small screens.

**Cart & ordering (Gate 4)**
- Cart line items now store **product + variant + quantity** (previously
  colour wasn't tracked in the cart at all).
- Checkout is explicitly labelled as an **order request, not a payment
  checkout** — copy says so twice (checkout page and confirmation page).
- The generated WhatsApp message includes product, colour, quantity, unit
  price, subtotal, delivery/pickup location, and customer contact details.

**Pricing & delivery (Gate 5)**
- All prices are carried over as **placeholders** and labelled
  "Indicative — confirmed on WhatsApp" everywhere they appear (cards,
  product page, cart, checkout, confirmation, footer). Nothing on the site
  implies a locked-in price.
- Delivery copy was stripped of any implied fee or zone — it consistently
  says delivery is arranged and confirmed on WhatsApp.
- Returns: "inspect your order at delivery/pickup before accepting" appears
  on the product page, cart, checkout, confirmation, and About page.
- Pickup location (Gakoromone Market, Meru) is on the product page, cart,
  footer, About, Contact, and in the page's structured data.

**Trust layer (Gate 6)**
- No invented reviews, ratings, or claims. Trust signals are the ones that
  are actually true: pickup address, WhatsApp-verified ordering, real
  social links, and the inspect-before-accepting policy.

**Mobile (Gate 7)**
- Rebuilt/verified against a 390×844 viewport: sticky mobile buy bar,
  60px+ tap targets on swatches and quantity controls, no horizontal
  overflow, category tabs and forms sized for touch and mobile keyboards.

**Technical hardening (Gate 8)**
- Added canonical URL, Open Graph tags, a Store/LocalBusiness JSON-LD
  block with the real address, and a proper favicon.
- Added `hash`-based routing (`#product?id=...`) so product pages are now
  shareable/bookmarkable links, and the back button works correctly.
- Converted clickable nav/menu/footer elements from `<span onclick>` to
  real `<button>` elements for keyboard accessibility; added
  `:focus-visible` states, a skip-to-content link, and ARIA labels on
  icon-only controls.
- Cart is sanitised on load: line items referencing a product/variant that
  no longer exists are dropped automatically instead of crashing or
  showing "undefined".
- Quantity is clamped to 1–99 everywhere it can be edited.
- Ran an automated functional pass (product render, variant switching, add
  to cart, checkout validation, order placement, stale-cart handling,
  image-file existence for every referenced variant) — all passing.

## What I could not respectfully generate for you

- **Final prices.** Every price in the code is a placeholder carried over
  from the old site and flagged as such. Find-and-replace in
  `index.html` under `var PRODUCTS=[` — each product has a `price:` field.
- **Exact case dimensions/weights.** Not visible in the source photos, so
  the spec block says "Not yet listed — ask on WhatsApp" rather than
  guessing. Once you have real measurements, add a `Dimensions` line to a
  product's `specs` array.
- **Group 09 and 13** genuinely mix a few different shell patterns under
  one supplier line in the sample photos — I labelled this honestly on the
  product page rather than pretending they're one uniform design.

## Updating prices later

Open `index.html`, search for `price:`, and update the number next to the
product you want to change (currency is KES, whole numbers, no commas).
No other file needs to change.
