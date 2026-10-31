# CLAUDE.md · 1000 Hills Ministry redesign

Context for any agent working on this project. Read this first.

## What this project is

A brand refresh and website redesign for **1000 Hills Ministry International**, a Christian ministry in Houston, Texas. Its main ministry is **Church in the Field**: every day, a different church from around Houston gathers on a small patch of grass to lead worship, share the Word, and share a meal (often pizza) with neighbors facing homelessness, addiction, and despair.

The old site is https://1000hills.org/Default.dmx. It is outdated and off-brand.

This is also a practice project for Joseph (the designer/developer), so explaining *why* behind decisions is welcome.

### Goals

1. **Website** (in progress, see Status below): a new SvelteKit site built on a shared set of design tokens.
2. **Physical and marketing materials** (later, not started): flyers, invitations, greeting cards, business cards, email newsletters. These should come from the same brand system. Volunteers will likely make them in **Canva**, so the plan is ready-made templates with fonts, colors, and logo already set, not rules people have to apply themselves.

One brand system feeds everything: tokens → website components → Canva brand kit and print templates.

## Status

**All four pages are built and live on the new design system:** `/` (homepage), `/about`, `/service`, `/contact`. The entire old `src/lib/components/OLD/` tree (the previous brand's components) has been **deleted** — nothing in the codebase references it anymore. If you're looking for "how did the old site do X," it's gone; check git history or https://1000hills.org/Default.dmx directly.

What's still open, roughly in priority order:
1. Real photos (everywhere is still a bracketed placeholder, e.g. `[Photo: ...]` — see Content rules).
2. Donation band copy: "[what else a gift covers]" is still bracketed in the homepage donate section (`$100 feeds everyone...`).
3. Ministry leader sign-off on all copy, the Scripture translation, and the EIN/501(c)(3) line (currently commented out in `Footer.svelte`).
4. Vector logo from the ministry.
5. Canva brand kit and print templates (not started).

## Audience and purpose

- **The website is for donors, volunteers, and partner churches**, not for the people the ministry serves (they are unlikely to use the site).
- **Calls to action, in order:** 1) Give, 2) Volunteer (including "host a night" for churches).
- **The problem being addressed is despair.** Core message: *God loves you, and so do we.*
- **3-second test:** a visitor should immediately think *"This is a Christian ministry, and they serve the homeless."*
- **Feel:** Christian, local, warm, humble, trustworthy. A community of neighbors, not an institution.
- **Must not feel:** clinical / healthcare, corporate, trendy, polished megachurch.

Brand statement to check work against:
> 1000 Hills is a local family of churches who show up every day, so no one in Houston faces despair alone.

**No dedicated `/volunteer` page.** Decided deliberately: `/contact` already covers it (director's direct phone/email + service times/map), and this is a one-person-point-of-contact ministry where a separate page would mostly repeat the same phone number. Every "Volunteer" button sitewide points to `/contact`, which leads with the director and service times before the more administrative mailing address/board info.

## The chosen design

**"Homepage · Lora + Nunito Sans, 10px corners"** is the final direction.

Reference files in `design/`:
- `homepage-desktop.html` / `homepage-desktop.png`: full desktop homepage
- `homepage-mobile.html` / `homepage-mobile.png`: 390px phone version

The reference HTML is static and uses inline styles, and **predates a few decisions made during the build — the SvelteKit code in `src/` is the source of truth where they differ.** Known differences from the reference:
- The donate band has **only one button** ("Give today") — no "Give monthly."
- The quote-from-an-attendee section and the "Bring your church for a night" section were **both replaced** by one new section: a Scripture verse (Matthew 25:35, ESV) paired with a food-serving photo placeholder. Reasoning: there's no confirmed attendee quote with permission to use, and checking the old site turned up zero evidence it ever made a "bring your church" ask anywhere (homepage, Contact Us, Where We Serve, Why We Serve all checked) — so that CTA was dropped rather than invented. The Scripture-verse idea was already flagged in this doc's "Open design notes" as available; using it here solves the content gap honestly instead of leaving a placeholder indefinitely.
- The "What happens in the field" section uses full `--space-xxxl` vertical padding (matching the hero/verse sections) rather than the reference's lighter padding — fixes a "choppy rhythm" issue where it and the donate band both felt thin next to their taller neighbors.

**Actual homepage section order, top to bottom:** nav → hero (eyebrow, headline, paragraph, Give today + Volunteer, selfie photo placeholder with caption) → layered hills divider → "What happens in the field" (3 cards) → navy donation band (centered heading/text left, "Give today" button right, side-by-side — a centered/stacked layout was tried and explicitly reverted, Joseph preferred the row) → "A word about serving" (photo placeholder + Matthew 25:35 verse, mist background) → footer.

### Design decisions

| Area | Decision |
|---|---|
| Heading font | **Lora** (Google Fonts), weight 600. Classic serif that matches the logo's serif and stays readable small. |
| Body font | **Nunito Sans** (Google Fonts), 400 / 700 / 800. Joseph finds it very easy to read. |
| Colors | Blues sampled from the logo, plus neutrals. Page background is **Mist** (light blue tint, not pure white). Navy carries text and buttons. |
| Rejected color | **"Dawn" peach accent: do not use.** It pulled away from the brand. |
| Corners | **10px** radius on buttons, cards, photos. No pill-shaped buttons, no square corners. |
| Buttons | Primary navy → hover pale haze with navy text and a navy edge (7.6:1). Secondary white with navy outline → hover solid navy. |
| Imagery | Real, candid photos of real people (selfies with the leader, volunteers, attendees; the grass lot; the pizza). No stock photos of people. Every photo slot sitewide is still a bracketed placeholder (dashed border, cloud-blue fill) — see Content rules. |
| Motif | Layered, soft hills from the logo, used as graphic dividers (`HillsDivider` component). |
| Voice | Plain, warm, first person plural ("we"), specific. Like a neighbor, not a nonprofit. |
| Heavy/dark sections | **Use navy full-bleed bands sparingly** — only where it's earned (the donate band, since Give is the #1 CTA). Learned this the hard way on `/about`'s documentary section: navy felt too heavy there, white felt empty/unstructured, side-by-side worked but Joseph preferred centered — landed on a **new light "feature" token** (`--color-surface-feature`, i.e. haze blue) for sections that should feel prominent without going dark. |
| Footer separation | The section before the footer isn't always a different color (mist CTAs are common), so the footer needs to read as a break on its own: **white background** (`--color-surface`, the lightest option) + a **thin 1px navy border-top** (not pale, not thick). A 2px navy border was tried and felt too heavy; a pale 1px border was invisible against mist. |

### Explored and set aside (don't re-propose without reason)

- B Merriweather, D Poppins, H Bitter: too round / playful.
- E Fraunces, G DM Serif Display: letterforms too quirky / too tall.
- I Montserrat: too techy. J Satisfy and other scripts: Joseph doesn't want script fonts.
- Libre Baskerville: fine, but more formal; Lora won on readability at small sizes.
- Pill buttons, square corners.
- A separate `/volunteer` page (see Audience and purpose above).
- A "bring your church" CTA + video on `/service` (no evidence it was ever a real ask; see homepage section above for the version that *is* kept).

### Open design notes

- Joseph once felt the light-blue homepage read slightly "healthcare/clinic." Explorations to push it toward "church first" included: faith language in the headline, a short Scripture verse (Lora italic), the logo's crosses as a motif, more navy and less pale blue, and real photos. **The Scripture-verse idea is now adopted** (see homepage section above and `/about`'s "What we believe"). He also liked **Cormorant Garamond** for an instantly "church" feel, and an **overcast, painterly sky** behind the hero (soft blue-gray gradient with subtle canvas texture), based on the misty-mountain invitation. Neither of those two is adopted yet.
- Real photos will change the feel more than anything else. Re-evaluate once they exist. Two real candidate assets already sit unused in `static/images/` (`food-line.jpg` looks like it could fill the new "serving" photo slot) but **haven't been vetted/wired in** — the project has deliberately stuck to placeholders everywhere rather than quietly introducing one real photo without the same scrutiny applied sitewide.

## Design tokens

File: `src/lib/styles/tokens.css`, imported once in `src/routes/+layout.svelte`.

**Two layers:**
1. **Palette tokens** (`--palette-navy`, `--palette-mist`, …): raw brand colors. Only used to define semantic tokens.
2. **Semantic tokens**, named by role, in Nord Design System style (`--{category}-{role}-{variant}`):
   - Color: `--color-background`, `--color-surface`, `--color-surface-tint`, `--color-surface-accent`, `--color-surface-placeholder`, `--color-surface-feature`, `--color-text`, `--color-text-weak`, `--color-text-link`, `--color-text-on-accent`, `--color-text-on-accent-weak`, `--color-accent`, `--color-accent-hover`, `--color-accent-active`, `--color-border`, `--color-border-strong`, `--color-border-decorative`, `--color-hill-*`, `--color-status-live`, …
   - Type: `--font-family-heading`, `--font-family`, `--font-weight-*`, `--font-size-xs` … `--font-size-xxxl`, `--line-height*`, `--letter-spacing-*`
   - Space: `--space-xxs` … `--space-xxxl` (4px base)
   - Shape: `--border-radius` (10px), `--border-radius-s`, `--border-radius-circle`, `--border-width*`
   - Layout: `--size-content-max` (1180px), `--size-text-max`, `--size-gutter`

Two tokens were added during the build, beyond what was originally specified — both because something genuinely didn't exist yet, per the "add a token first" rule:
- `--color-surface-feature` (= `--palette-haze`): a light-blue band for sections that need to feel prominent without going full navy. Navy text on it is 7.6:1. Used on `/about`'s documentary section.
- `--color-status-live` (`#4ade80`, green): the "happening now" pulsing dot in `ServiceTimes`.

**Rules:**
- **Components use semantic tokens only.** No hex values or raw px colors in components. If something is missing, add a token to `tokens.css` first (this has happened twice — see above — it's expected, not a violation, as long as it's a real gap).
- Never use `--color-hill-*`, mid blue, or sky blue for text on white (they fail contrast). Navy and deep blue do the text work. Contrast notes are in the token file comments.
- When text sits on sky blue, it is navy (`--color-text-on-accent-hover`), never white.
- The old "Example usage" section at the bottom of `tokens.css` has been removed; the real components are the reference now.
- **Print:** the same tokens should become the Canva brand kit (colors as hex) and print specs. CMYK values still need to be defined and proofed with a printer. Not started.

## Code conventions

- **SvelteKit, Svelte 5 (runes: `$props`, `$state`, `$derived`, `$effect`, snippets), TypeScript.**
- Components live in `src/lib/components/` (no `OLD/` subfolder anymore — it's deleted):
  - `Text`: `as`, `size` (xs/s/m/l), `tone` (default/weak/link/on-accent/on-accent-weak), `weight` (regular/strong/active), `variant` (body/eyebrow/caption), `measure`
  - `Heading`: `level` (1–6, sets the tag), `size` (xxxl/xxl/xl/l, sets the look), `tone` (default/on-accent), `balance`
  - `Button`: renders `<a>` with `href`, otherwise `<button>`. `variant` (primary/secondary/inverse/outline-inverse), `size` (s/m), `block` (true / `"mobile"`)
  - `Nav`: `links`, `giveHref`, `logoSrc`. Animated hamburger (bars morph to X) on mobile with a `max-height` panel transition. Used once in `+layout.svelte`.
  - `Footer`: `logoSrc`, `tagline`, `phone`, `email` (optional, no default), `facebookUrl`, `serviceName`, `street`, `cityLine`, `legalName`, `ein`. Three columns (brand+copyright / Contact incl. Facebook / service address with a directions link) that stack on mobile. The EIN/501(c)(3) line is commented out in the markup pending confirmation — don't delete it, just uncomment when confirmed. Used once in `+layout.svelte`, appears on every page.
  - `HillsDivider`: `groundColor` (CSS color, default `var(--color-surface)`) — the front hill layer's fill, set to match whichever section follows. The wavy SVG motif, extracted so it's not copy-pasted per page.
  - `VideoEmbed`: `provider` (youtube/vimeo), `id`, `title`. Vimeo embeds directly; YouTube shows a lazy poster + play button until clicked (avoids loading YouTube's tracking scripts upfront).
  - `ServiceTimes`: `id`, `eyebrow`, `heading`, `intro`, `locationName`, `street`, `cityLine`, `schedule`, `durationMinutes`, `timeZone`, `showMap`. Computes a live "Happening now" / "Next service: ..." badge client-side (so the server's clock never shows stale text) and embeds a keyless Google Maps iframe when `showMap` is true. Used on `/service` and `/contact`.
- Layout-level things (Nav, Footer, skip link) go in `+layout.svelte`; page content in each `+page.svelte`.
- **No shared `Card`/`Section` wrapper components exist.** Every page hand-rolls its own section padding/max-width using the same repeated pattern: `max-width: var(--size-content-max); margin-inline: auto; padding: var(--space-xxxl) var(--size-gutter);` (or similar), with a `@media (max-width: 900px) { padding-inline: var(--space-m); }` override. Cards are just `background: var(--color-surface-tint); border-radius: var(--border-radius);` divs in a `flex-wrap` list. This was a deliberate choice to avoid introducing abstraction before a real pattern was proven — if you're about to copy this pattern a 4th or 5th time, consider extracting a `Section`/`Card` component instead of copying again.
- Extra attributes pass through to the underlying element (`target`, `rel`, `aria-*`, `onclick`).
- **External links that open a new tab:** `target="_blank" rel="noopener noreferrer"` plus visually hidden text "(opens in a new tab)".
- Breakpoints: **900px** (nav collapses to a menu, sections tighten side padding) and **480px** (buttons go full width with `block="mobile"`). CSS variables can't be used in media queries, so these are written as numbers.
- Accessibility: real `<a>` / `<button>` elements, visible `:focus-visible` outlines, 44px minimum tap targets, `prefers-reduced-motion` respected, text contrast at least 4.5:1. `inert` should be scoped carefully — it was once applied to a nav panel wrapper that was `display: contents` on desktop, which silently made every nav link unclickable there; prefer `visibility`/`opacity` + a scoped media query over `inert` when the element's `display` changes by breakpoint.
- Donation link: `https://www.paypal.com/donate/?hosted_button_id=CMTD9CKPZFAJU`

## Content rules

- **Never invent facts.** Use [bracketed placeholders] for anything not confirmed: donation amounts (currently "$100 feeds everyone... [what else a gift covers]" — the dollar figure is confirmed, the second part isn't), partner church names, times, names in photos, contact info.
- **Quotes and photos of attendees only with their permission.** Show people with dignity, as part of the community, never as objects of pity. There is currently no confirmed attendee quote — the homepage uses a Scripture verse instead rather than leaving an indefinite placeholder (see "The chosen design" above).
- **Scripture: using ESV in practice**, verified against esv.org (not quoted from memory) each time: **1 Corinthians 15:3–4** (`/about`, "What we believe") and **Matthew 25:35** (homepage, "A word about serving"). This is a practical default while the **ministry's actual preferred translation is still unconfirmed** — if they specify a different one, both verses need updating.
- Copy written so far is based on Joseph's description of Church in the Field. Have the ministry leader check it before launch.
- When adding new unconfirmed content, verify against external sources (WebSearch/WebFetch) rather than trusting memory where accuracy matters (this applies to Scripture, and would apply to e.g. quoting the old site verbatim).

## Known assets

- **Logo:** `static/images/1000hills-logo.png`. Only 218×145px, so it is blurry on high-res screens. **Need a vector (SVG/AI/EPS/PDF) from the ministry.** The logo is a serif "1000 Hills" with three crosses on layered blue hills and a navy "Ministry International" band.
- **Invitation** ("Coffee and Dessert"): made in Canva. Brush script + rounded sans, misty blue mountain photo. The photo is likely Canva-licensed, so **don't reuse it on the website**.
- **Real photos exist in `static/images/`** (`field.jpg`, `field-2.jpg`, `field-3.jpg`, `food-line.jpg`, `license-plate.jpg`, `prayer-circle.jpg`, `prayer-circle-2.PNG`, `service.jpg` through `service-4.jpg`, `services-banner.jpg`, `tent.png`) — these were used by the now-deleted OLD components but are **not wired into the new design**. Every photo slot on the new site is still a bracketed placeholder on purpose (see Open design notes) — don't start using one of these files without checking with Joseph first, even though they look like plausible real candidates (e.g. `food-line.jpg` for the homepage's new serving-photo section).
- Confirmed real contact/service facts already in use on the site (not placeholders): Director **Lee Whitlock**, `(281) 787-8275`, `lee.whitlock1993@gmail.com`; mailing address **1000 Hills Ministry, PO Box 591262, Houston, TX 77259**; Board of Directors (Pat Lavergne – Secretary/Treasurer, Lisa Degner, Richard Degner, Dr. Joseph McCollum, Joel Mueller); service location **2800 St. Charles St., Houston, TX 77004** ("the big vacant lot"), Mon–Sat 7:30 PM / Sun 3:00 PM; Facebook `facebook.com/1000Hills`; the documentary (Vimeo ID `16186350`, credited to Homer Films); legal name "1000 Hills Ministry International Inc." and EIN `51-0666528` (this last one specifically is commented out on the live site pending confirmation — don't treat "commented out" as "wrong," just "not yet confirmed for public display").
- Old site content (for reference, old site is otherwise superseded): daily outdoor church services and meals in Houston; Facebook, Vimeo, and PayPal links. Old taglines: "Serving the people & bringing Houstonians together as one," "Feeding body & soul in the fight against poverty, homelessness & addiction." Checked thoroughly for a "bring your church to lead a service" CTA — **it does not exist anywhere on the old site.**

## Next steps

1. **Get from the ministry:** vector logo, real photos (with permission), confirmation on the $100 "what else a gift covers" line, partner church names, Scripture translation, EIN/501(c)(3) display confirmation, and whether the name comes from Psalm 50:10 ("the cattle on a thousand hills").
2. Once real photos exist, swap them into the placeholder slots sitewide and re-evaluate the "healthcare/clinic" feel question from Open design notes.
3. If an attendee quote with confirmed permission becomes available, decide whether it replaces or supplements the Matthew 25:35 section on the homepage.
4. Set up the Canva brand kit (Lora, Nunito Sans, palette hex values, logo files). Not started.
5. Make Canva templates: invitation, flyer, greeting card, business card, email newsletter. Not started.
6. Write a one-page brand guide for volunteers. Not started.
7. If a `Card`/`Section` pattern gets copied again, consider extracting shared components (see Code conventions).

## Files

```
CLAUDE.md
design/                       ← reference for the chosen homepage (code is source of truth where they differ)
src/lib/styles/tokens.css
src/lib/components/{Text,Heading,Button,Nav,Footer,HillsDivider,VideoEmbed,ServiceTimes}.svelte
src/routes/+layout.svelte     ← Nav + Footer + skip link
src/routes/+page.svelte       ← homepage
src/routes/about/+page.svelte
src/routes/service/+page.svelte
src/routes/contact/+page.svelte
static/images/1000hills-logo.png
static/images/*.jpg, *.png    ← real photos, not yet wired in (see Known assets)
```

All design explorations (font pairings, palette, homepage rounds, "church first," traditional, and hero sky variations) live on a Claude design canvas called "1000 Hills Type Pairings" in Joseph's claude.ai artifacts.
