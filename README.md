# Seartec Microsite (staging)

A one-page microsite for **Seartec**, promoting Month-to-Month printer and multifunction device rentals. Built by Conversion Advantage and hosted on GitHub Pages.

**Live staging URL:** https://conversionadvantage00.github.io/seartec-staging/  
**Repository:** https://github.com/conversionadvantage00/seartec-staging

## What's on the page

| Section | Purpose |
|---|---|
| Hero | Month-to-Month headline, key stats, CTAs to quote and plan finder |
| Why Seartec | Zero risk, everything included, sized pricing, track record since 1968 |
| Plan finder | 3-step picker: industry → team size → recommended plan |
| 30-day free trial | How the trial works |
| Good to know | Term, price, delivery and gap cover details |
| Expertise | "Printer problems? Ask the people who fix them" |
| Reviews | Client testimonials |
| Branches | The five branches with click-to-call numbers |
| Get a quote | Quote request form |

## Files

```
/ (repo root)
├── index.html   # The whole site: HTML, CSS, JS and images (base64) in one file
└── README.md
```

The site is fully self-contained. The only external request is Google Fonts (Barlow). There is no build step and nothing to install.

## Deploying

1. Replace `index.html` in the root of this repo and commit to `main`.
2. GitHub Pages republishes automatically, usually within a minute or two.
3. Hard-refresh the staging URL (Ctrl/Cmd + Shift + R) to bypass the cache.

Pages must be enabled under **Settings → Pages**, source `main` / root.

To preview locally, just open `index.html` in a browser.

## Before going live

- [ ] **Connect the quote form.** It currently validates and shows a thank-you message, but **does not send the enquiry anywhere**. Hook it up to Formspree, Netlify Forms, a CRM webhook or similar (see the `quoteForm` submit handler near the bottom of `index.html`).
- [ ] **Remove the `noindex` tag.** The page carries `<meta name="robots" content="noindex, nofollow">` so the staging copy isn't indexed by Google. Delete that line once the site is on its final domain.
- [ ] **Update `og:url`** to the final domain, and add an `og:image` for link previews.
- [ ] **Social links.** Facebook and LinkedIn currently point to the platform home pages; swap in Seartec's profile URLs.
- [ ] Check branch phone numbers, the WhatsApp number (+27 21 404 2800) and `info@seartec.co.za` with the client.

## Editing notes

- Colours and type sizes are CSS variables in `:root` at the top of the file (cyan `#00afd7`, ink `#1e1e21`, Barlow font).
- Images are embedded as base64, which is why the file is ~3.3 MB. To make edits easier or the page lighter, images can be moved into an `assets/` folder and referenced by path.

---

© Seartec. Site by Conversion Advantage.
