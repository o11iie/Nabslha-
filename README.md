# NABSLHA website

Single-page website for **NABSLHA — National Black Speech, Language & Hearing Association**:
*"Every voice deserves to be heard."*

The whole site is one self-contained file, [`index.html`](index.html). All CSS and JavaScript are inline, Three.js r128 (for the 3D "voice halo" in the hero) is inlined, and the NABSLHA logo is embedded. There is no build step or framework. Enquiries are collected with Netlify Forms. The only external request is for the Google Fonts stylesheet (Fraunces, Inter, Space Mono), and every font has a system fallback.

## Viewing and deploying

- **Locally:** open `index.html` in any modern browser.
- **Hosting:** upload `index.html` to any static host. For GitHub Pages, go to *Settings → Pages → Deploy from a branch* and choose the branch with the `/ (root)` folder.

## Source of truth

The site reproduces the supplied final demo (`nabslha-website-final.html`) and follows the V2 build prompt's rules for architecture, accessibility and behaviour. The prompt was written against an earlier demo file, so it disagrees with the final demo in a few places. The prompt's own rule says that when they conflict, **the demo HTML wins**, so this build follows the demo on these points:

| Topic | V2 prompt | Final demo (followed here) |
|---|---|---|
| Executive Committee | 5 members | **6 members**, including **Dr Ayanda Gina** (Marketing & PR Coordinator) |
| "Apply for membership online" and "Pay your membership fee" cards | Present | **Not included** (see below) |
| Brand bar under the hero text | Present | Not in the markup (style kept, unused) |
| Footer "Apply online" link | Present | Not included (it went with the online application form) |
| Copy details | e.g. "hearing-aid acousticians", short programme blurbs | Demo wording, e.g. "hearing aid acousticians", full programme descriptions |

**Confirmed decision:** the online membership application form and the payment gateway (PayFast) are no longer part of the website. The site has no cards or footer link for them, and the demo's leftover styles for them have been removed. Membership buttons lead to the enquiry form. The form collects interest and renewal enquiries; it does not complete membership registration or payment.

## What changed relative to the demo

Section by section, the build is pixel-identical to the demo at 1440, 1000, 768 and 430 px, with these exceptions.

**Visible fixes for defects in the demo:**
- **Hero canvas:** the demo sized the 3D canvas to the window and let it overflow the hero. On retina screens it rendered at 2–3× size, which pushed the halo off-centre, and on phones it was stretched to the full window height. It now always fills the hero exactly, capped at DPR 2.
- **Tablet navigation (721–1170 px):** the logo chip collapsed to an empty pill and "Become a member" was cut off. The nav now compacts in steps: it hides the wordmark subtitle, then the wordmark and some link padding, and at 721–940 px it hides the nav button. The logo never shrinks.
- **Mobile menu:** it is now full-screen and scrollable. On short phones (e.g. 320×568) the last items used to be unreachable.
- **"Our fields" tiles:** below about 385 px the captions were clipped ("Speech-Language Therapy" showed only "Therapy"). The tiles are now single-column at the demo's smallest breakpoint (≤420 px).
- **Short screens:** the hero copy no longer slides under the fixed nav (e.g. iPhone SE, landscape phones). Where the content already fitted, its position is unchanged.
- **Featured "Professional" tier:** the button label was white on gold (2.6:1 contrast). It is now navy, like every other gold button.
- **Very wide screens (≥1800 px):** the marquee loop used to show an empty gap. It still passes one 8-item cycle every 34 s.
- **Core Values hover:** the demo's CSS and the brief both call for the icon to lift slightly on hover, but in the demo it never moved (the rule targeted an inline wrapper). It now lifts 3 px.

**Behaviour and accessibility (no visual change):**
- Landmarks: `<main>`, a mobile `<nav>` and a skip link. There are visible `:focus-visible` rings (royal blue on light surfaces, gold on navy), and the heading levels are in order.
- The closed mobile menu is out of the tab order. When open, the page behind it is inert, Escape closes it, and it closes itself if the window widens past 720 px.
- In-page links move keyboard focus to their target. The active nav link is correct in both scroll directions and clears at the hero.
- Biographies are hidden from assistive technology while collapsed and animate to their measured height, so long ones are never clipped.
- The form uses native required-field and email validation and announces submission results through its `role="status"` region.
- The 3D loop pauses off-screen and in hidden tabs, including the demo case where it restarted off-screen after a tab switch. It resumes without jumping. If the GPU context is lost, the static SVG fallback shows.
- Without JavaScript, all content stays visible. There are Safari prefixes for `backdrop-filter`, `vh` fallbacks for `svh`, and a proper table header structure.

## Production enquiries and deployment

The static `membership-interest` form uses Netlify Forms, a honeypot and required consent. JavaScript shows a sending state and displays success only after an accepted HTTP response; failures preserve entered details for retry. Without JavaScript, native form submission leads to `/thank-you`. Submissions are reviewed in the Netlify site's Forms area, including the spam folder. No email notification recipient has been configured or verified by this update.

The form activation marker enables detection through the deployment workflow. A manual deploy must also have form detection enabled on the site. Deploy the static files without a build command. Confirm that `membership-interest` appears in Forms after deploying.

All membership calls to action use the form instead of requiring a mail application. The footer routes enquiries to the form instead of relying on an unverified mailbox. The privacy notice describes what the enquiry form collects and how visitors can request access, correction or deletion.

The palette keeps gold for decoration and uses darker gold for small text on light surfaces. Search files, canonical metadata and a PNG social sharing image target `https://nabslha.org.za/`. Both the apex domain and `www` must point to Netlify and have valid HTTPS before that custom domain can serve the site.

## Editing notes

- The inlined Three.js r128 bundle sits on one very long line near the end of the file. Edit the rest of the file normally.
- The Core Values and Leadership sections are plain HTML (the demo built them with JavaScript), so text edits happen directly in the markup.
