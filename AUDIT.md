# allears.nz site audit

Date: 2026-09-09. Covers all 61 HTML pages, the CSS/JS, the image
library, sitemap and robots.

Severity is **fix now** (broken, wrong, or actively costing traffic),
**should fix** (real problem, not urgent), **nice to have**.

Everything marked APPLIED is already committed to `main`. Everything
marked `TODO(will:...)` needs a fact only you have, so it was left
alone rather than guessed at.

---

## Top findings

1. **No `<meta name="viewport">` on any page.** All 61 pages rendered
   at desktop width on phones and the CSS media queries never fired.
   The site had breakpoints at 1264 / 1064 / 784 / 564px that could
   not run. Single worst defect found. **fix now — APPLIED**
2. **The address was wrong site-wide.** Postcode 8011 (Central City)
   instead of 8023, and no suburb, in the LocalBusiness JSON-LD on 59
   pages and the visible NAP on 57. contacts.html said "Christchurch
   Central City". Bad NAP consistency hurts local ranking directly.
   **fix now — APPLIED**
3. **All 61 homepage gallery tiles were `href="#"`.** 23 showcase
   pages existed and got zero link equity from the homepage. Three had
   no inbound link from anywhere. **fix now — APPLIED**
4. **The DJ pages advertised gear that does not exist.** "CDJ-3000X"
   is not a Pioneer DJ product; the DJM-V10 is not stocked. Both
   appeared in the meta description, OG tags, Service schema and FAQ
   schema on three pages, while the visible page showed different
   gear. Classic Search Console structured-data mismatch. **fix now —
   APPLIED**
5. **404.html used relative paths.** GitHub Pages serves it for any
   missing URL at any depth, so `/blog/anything` rendered with no CSS,
   no JS and broken nav. It was also set to `index, follow`. **fix now
   — APPLIED**
6. **Keyboard focus was globally disabled.** `plugins.css:11` sets
   `:focus { outline: 0 }` and the reset strips outlines everywhere.
   No focus ring on any link or button on any page. WCAG 2.4.7 fail.
   **fix now — APPLIED**
7. **Literal `\'` rendering as text** on staging, truss and festoon
   pages ("What\'s Included"). **fix now — APPLIED**
8. **~28 MB of unreferenced images are committed and deployed**,
   including the entire `docs/events/full-res/` directory (18 MB, 28
   files, referenced by nothing). **should fix — NOT APPLIED, see
   below**
9. **Live images are enormous.** A 3.3 MB / 5500px hero on the festoon
   page, a 2.0 MB / 7008px-tall background on a showcase page, ~400 to
   600 KB product thumbnails displayed at 200px. Dominates mobile LCP.
   **should fix — NOT APPLIED**
10. **Three DJ pages are byte-identical clones.** `cdj-hire`,
    `dj-equipment-hire` and `dj-backline-hire` differ only in the
    title, h1 and canonical. Two of them have zero inbound links.
    Google will pick one and suppress the rest. **should fix —
    decision needed, see below**

---

## What changed, by page

### Site-wide (all 61 pages) — APPLIED
- Added the viewport meta tag.
- Corrected the address to 20 Southwark Street, Sydenham,
  Christchurch 8023 in JSON-LD and the visible NAP.
- Removed `<meta name="keywords">`. Ignored by search engines, empty
  on 30 pages.
- Gave the header logo link an accessible name.
- Restored `:focus-visible` outlines in style.css.

### index.html — APPLIED
- Wired 60 gallery tiles to their showcase or service page.
- Two tiles had alt text describing the wrong event (a HIIT for Hope
  photo labelled Twominds, and the reverse). Swapped.
- Renamed the Flatmate tile to **Interlude Presents Flatmate**,
  caption title-cased to "Full Production & Crew" to match the others.
- Fixed a duplicated dev comment and inconsistent caption casing.

### contacts.html — APPLIED
- The page had **no h1** at all. The hero `<h2>` is now an `<h1>`.
- Visible address corrected from "Christchurch Central City,
  Christchurch 8011".
- Google Maps link now includes the suburb.
- Sidebar button said "About Page"; the nav calls it "Services".

### 404.html — APPLIED
- All paths root-relative, `noindex`, real title, plain error copy.

### DJ pages (backline / cdj / dj-equipment / dj-hire) — APPLIED
- Removed the phantom CDJ-3000X and DJM-V10 from meta, OG, Service
  schema and FAQ schema across three pages.
- FAQ schema now matches the visible FAQ.
- Fixed a doubled keyword in the cdj page's keywords meta.
- Corrected "Pioneer CDJ-3000" to "Pioneer DJ CDJ-3000" in alt text.
- Added `rel="noopener"` to the unit20.nz links.

### Audio pages (speaker / pa / av-hire) — APPLIED
- "2000W" now reads "2000W peak" (it is QSC's peak rating, not
  continuous).
- Trimmed two over-length meta descriptions.
- Filled three empty content-image alts.
- Removed a sentence that said nothing ("AV is a broad term and people
  use it to mean different things").

### Lighting pages — APPLIED
- **uplight, astera and festoon were orphans.** The lighting parent
  page did not link to any of its three children. Now linked.
- The festoon page's "uplighting" link pointed at the parent lighting
  page instead of the uplight page. Fixed.
- Festoon FAQ schema rewritten to match the visible answers.
- Dropped the unverifiable "IP66, 20+ hour battery" claim from the
  uplight meta description (see TODOs).
- "Pricing" heading renamed "Hire Rates" to match sibling pages.

### Staging / truss / LED screen — APPLIED
- FAQ schema on staging and truss did not match the visible page at
  all. Now aligned. This also removed several **safety claims that
  existed only in the schema** and had never been reviewed as visible
  copy (see TODOs).
- Trimmed two over-length meta descriptions.

### Event-type pages — APPLIED
- The wedding page numbered two services "03." in a row.
- Rewrote the worst filler on the school ball page.
- School ball FAQ schema aligned to the visible answer.
- Filled empty alts on three content images.

### Blog — APPLIED
- `how-much-does-pa-hire-cost` quoted **$70** for a QSC K12.2 when the
  service page charges **$75**, and quoted prices without GST when
  every price on the site is +GST. Corrected.
- Deleted the self-undermining line "these figures may not reflect
  current pricing".
- "Horncastle Arena" has been Wolfbrook Arena for years. Updated.

### Showcase — APPLIED
- Address and viewport fixes only. Checked all 23 for wrong-event
  copy-paste errors and canonical/og:url mismatches: **none found**,
  which is better than expected.

---

## Not applied, needs your call

### Facts I could not verify
- `TODO(will:)` **hello@allears.nz vs will@allears.nz.** The brief says
  will@; the site uses hello@ in 121 places, consistently, including
  the Google-facing JSON-LD. Left alone deliberately. Switching the
  public contact address on a live site is a business decision and if
  hello@ is the monitored inbox, changing it loses enquiries. Say the
  word and it is a one-line sweep.
- `TODO(will:)` **Geo coordinates in the JSON-LD** are -43.532,
  172.636, which is the central city, not Sydenham. Needs the real
  lat/long for 20 Southwark Street.
- `TODO(will:)` **Opening hours** Mon-Fri 9am-4pm appear in schema and
  the NAP on every page but were not in the brief.
- `TODO(will:)` **The XDJ-RX2 is $160/unit while the CDJ-3000 flagship
  is $140/unit.** Looks like an inverted or stale rate.
- `TODO(will:)` **The new RX3 is priced per weekend while every other
  item on that page is per unit.** Priced as you specified, but the
  page now mixes two hire periods in one row.

### Prices that disagree with the crew equipment register
The site and the register do not match on six items. Site first:
uplight $30 / register $25, Astera pack $300 / $250, Thunder P60 $70 /
$60, festoon 15m $30 / $45, fairy lights $15 / $20, festoon pole $12 /
$12.50. The festoon one is the concerning direction: the site charges
$15 less than the register. `TODO(will: reconcile)`

### Safety and load claims
These are on rigging and staging pages and carry real liability. All
left exactly as they are on the visible page; several were removed
from the JSON-LD where they had never been visible at all.
- `TODO(will:)` "rated for hanging speakers, lighting fixtures, LED
  screens" with no figure or series cited.
- `TODO(will:)` "our crew are trained for working at height".
- `TODO(will:)` "we work with structural engineers to ensure
  compliance".
- `TODO(will:)` "Ground-supported truss totems and smaller rigs
  usually don't need engineering sign-off" — this is safety advice to
  the public.
- `TODO(will:)` "For simple ground-supported rigs under standard load
  limits" — "standard load limits" is not defined anywhere.
- `TODO(will:)` The LED screen page says "No scaffolding, no truss, no
  structural engineer" for a 4m x 2.5m outdoor screen. Wind loading.
- `TODO(will:)` Is the truss genuinely Eurotruss F34? It is asserted in
  the title, schema and every alt tag.

### Claims that read as invented
- `TODO(will:)` Client names: Summit Touring, Offline Collective, Dine
  for a Cure, Red Bull, Rhythm and Vines, Ensoc, Lads Without Labels.
  Most appear in the showcase so are probably real, but they are named
  as credentials on service pages too.
- `TODO(will:)` School ball venue credits: Rochester, Rutherford Hall,
  Tupuanuku. These are also in the meta description.
- `TODO(will:)` "We started as a student-run operation."
- `TODO(will:)` "Up to 400 silent disco headsets."
- `TODO(will:)` "Up to 4km of festoon in stock", repeated five times.
  The register shows 20 x 15m strings, which is 300m.
- `TODO(will:)` about.html stats: "100+ Events Delivered", "20+
  Different Venues".
- `TODO(will:)` faq.html said "We've never had a show stop because of
  a gear failure" — removed, since it sits next to a terms clause
  disclaiming uninterrupted service. Put it back if you'll stand
  behind it.
- `TODO(will:)` faq.html claims gear is "fully insured" with a
  certificate of currency available, which is firmer than terms.html
  clause 12.
- `TODO(will:)` faq.html mentions generators. They appear nowhere else
  on the site.
- `TODO(will:)` Venue names in blog posts I could not verify:
  "Trevenna", "The Atrium".
- `TODO(will:)` Astera model. The register says AX1 tubes; the page
  keywords target "Astera Titan Tube". Different products.
- `TODO(will:)` LED screen "runs off a standard 15A supply".

### Inventory contradictions
- `TODO(will:)` The school ball page sells Springtop and pole
  marquees plus an inflatable party cube. The marquee page sells
  pagodas, popups and clear roof. They describe different stock.
- `TODO(will:)` The blog post `what-size-marquee-do-i-need` is built
  around 6x9m to 10x18m frame marquees which you do not appear to
  carry. It is the one post I would call mostly filler, and it will
  generate quote requests you cannot fill.
- `TODO(will:)` Two product images are byte-identical and shown as
  different products (the 1.6m and 2m DJ tables).
- `TODO(will:)` staging schema listed a 0.5m x 1m deck with no price
  card and no image.

### Structural, deliberately not done
- **28 MB of unreferenced assets.** `docs/events/full-res/` (18 MB) is
  referenced by nothing, but it looks like a deliberate archive of
  originals, so deleting it was not my call. Also orphaned:
  `events/truss1.png` (2.2 MB), `events/rigging1.png` (1.9 MB),
  `images/products/festoonhireheroimage.webp` (3.3 MB, a duplicate of
  the festoon hero), `images/clients/Screenshot 2025-07-12 at 2.42.00
  PM.png`, and `docs/images/site.webmanifest` (a broken duplicate with
  an empty name and icon paths that do not resolve; the root one is
  the good one and is what every page links).
- **Image compression.** Needs judgement on quality; happy to do it.
- **No `<img>` on the site has width/height** except the new RX3. 292
  images, all causing layout shift. Mechanical but needs reading every
  image's real dimensions.
- **No skip link** on any page.
- **Tap targets are small**: 12px unpadded nav links, a 38px
  hamburger, against a 44px guideline.
- **Header, nav and footer are copy-pasted into all 61 pages.** Actual
  drift today is whitespace only, so this is not yet a bug. Options if
  you want to fix it: a lint script that hash-compares the blocks and
  fails on drift (near-zero risk, keeps GitHub Pages publishing from
  docs/ untouched); or a small build step assembling partials into
  docs/ (better long-term, but docs/ becomes a build artifact). Do not
  use client-side JS injection: the nav would be invisible to crawlers
  and to no-JS users. **Recommendation: lint script now, build step
  only when the shared chrome actually starts changing.**
- **The DJ page clones.** Either canonical `cdj-hire` and
  `dj-equipment-hire` at `dj-backline-hire`, or genuinely
  differentiate them. Suggested split: backline keeps the per-unit
  rate card, cdj-hire becomes a CDJ-3000/2000NXS2 deep dive, and
  dj-equipment-hire becomes the broad "everything a DJ needs" page.
- **speaker-hire and pa-hire overlap** the same three products at the
  same prices. Suggested split: speaker-hire owns per-unit dry hire
  rates, pa-hire owns complete operated systems by event size.
- **The contact form is commented out** in contacts.html. The submit
  handler is still live in scripts.js so it would work, but the
  formsubmit.co endpoint needs checking and the inputs are
  placeholder-only with no labels. Restore it or delete the dead
  markup.
- **terms.html is orphaned.** Nothing links to it. T&Cs nobody can
  reach are weak contractually. Suggest linking it from the footer
  policy box on every page.
- **The blog is nearly orphaned too** — one body link from about.html
  and nothing else. No nav or footer entry.
- **sitemap lastmod is stale** on most entries (May/June, against real
  edits through August). Either update on deploy or drop lastmod.
- `TODO(will:)` terms.html says "Unit 20, 20 Southwark Street", which
  no other page says. Also: no bond clause, no PPSA clause, no privacy
  policy page despite clause 15.2, and the 50% deposit in 5.1 vs the
  30% late-cancellation fee in 8.1 do not say whether they stack.

---

## Competitor gaps: tes.nz and gravityevents.co.nz

**Neither competitor publishes hire prices anywhere** (one exception:
Gravity lists Starlink from $400+GST). Everything else is quote-gated.
All Ears publishing rates is a genuine wedge and worth leaning into.

**Services they have pages for that we do not:** conference AV,
projector hire, microphone hire, live streaming and hybrid events,
power distribution, theatre and school productions, afterballs,
birthdays and private functions, pipe and drape, silent disco, LED
dance floor, photo booths, photography and videography, Starlink. TES
also runs an entire second business in AV installation and equipment
sales.

**Page types they have that we lack:** a single full gear catalogue
page (both have one), a dedicated testimonials page (TES's, with 12
named clients including SailGP and Ngāi Tahu, is the strongest trust
asset on either site), a careers page, a team page with faces, and a
quote form that pre-qualifies with enquiry-type dropdowns.

**Where they are weak and we can win:** neither has a delivery and
pickup information page, TES's FAQ dodges dry-hire specifics, and
Gravity's Christchurch presence is thin (two staff, no local address).
Public pricing plus a Sydenham warehouse plus dry-hire clarity is the
opening.

### Suggested new pages, ranked

1. `hire-price-list-christchurch.html` — the full catalogue with
   prices. Nobody else publishes any. Natural hub page.
2. `conference-av-hire-christchurch.html` — both competitors have one.
3. `projector-hire-christchurch.html` — we already blog LED vs
   projector, so we clearly do this.
4. `microphone-hire-christchurch.html` — high-intent standalone query.
5. `afterball-hire-christchurch.html` — we already own the school ball
   niche; this is the adjacent purchase by the same buyers.
6. `party-hire-christchurch.html` — no new stock needed.
7. `delivery-and-pickup.html` — nobody has one, and it converts
   dry-hire fence-sitters. Now that delivery is $50+GST each way with
   24/7 self-service collection, this page writes itself.
8. A testimonials page — the 23 showcases are the raw material.
9. `power-distribution-hire-christchurch.html` — only if we hire
   distros.
10. `theatre-production-hire-christchurch.html`, 11.
    `festival-production-christchurch.html`, 12.
    `live-streaming-christchurch.html` — all conditional on real
    credits or stock.

Not suggested, no evidence we offer them: silent disco, LED dance
floor, photo booths, Starlink, photography, AV installation.
