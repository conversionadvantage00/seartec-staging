# Seartec Microsite

A static one-page microsite for Seartec. It's plain HTML, CSS and JavaScript, with no build step. It's hosted on GitHub Pages, and the quote form sends through Formspree.

```
.
├── index.html     # the whole page (markup, styles, scripts)
├── img/           # every image the page uses
├── .nojekyll      # tells GitHub Pages to serve files as-is
└── README.md
```

## Run locally

Open `index.html` in a browser. Or, to match how GitHub Pages serves the site:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploy to GitHub Pages

1. Push these files to the root of a repo (e.g. `seartec-microsite`) on `main`.
2. In the repo, go to **Settings → Pages → Build and deployment**. Set Source to *Deploy from a branch*, Branch to `main`, and folder to `/ (root)`.
3. The site goes live at `https://<user-or-org>.github.io/<repo>/` after about a minute.

All asset paths are relative (`img/...`), so the site works both at a sub-path and on a custom domain.

### Custom domain (later)

Add a `CNAME` file containing the domain (e.g. `quote.seartec.co.za`). Then point DNS at GitHub Pages: use a `CNAME` record to `<user-or-org>.github.io` for a subdomain. Finally, tick **Enforce HTTPS** under Settings → Pages. Add the new domain to Formspree's allowed domains too (see below).

## Quote form (Formspree)

### Setup

1. Create a form at [formspree.io](https://formspree.io). Set its target email to **enquiries@seartec.co.za** and verify that address.
2. Copy the form ID from the form's Integration tab (the `xxxxxxx` in `https://formspree.io/f/xxxxxxx`).
3. In `index.html`, find `FORM_CONFIG` (search for `FORM CONFIG: edit here`) and replace `YOUR_FORM_ID`:

   ```js
   endpoint: "https://formspree.io/f/xxxxxxx",
   ```
4. Recommended: in Formspree form settings, turn on **Restrict to domain** with the live domain, so the endpoint can't be used from other sites.
5. Send a test submission from the live site. Formspree asks you to confirm the first one.

Until the ID is set, the form validates as normal but shows "Form not connected yet" instead of sending.

### What gets sent

| Field | Source | Notes |
|---|---|---|
| `first`, `last` | `#f-first`, `#f-last` | required |
| `email` | `#f-email` | required; Formspree uses it as Reply-To |
| `phone` | `#f-phone` | required (7+ digits) |
| `company` | `#f-company` | |
| `branch` | `#f-branch` | Cape Town, Paarl, Johannesburg, Pretoria, Durban, Not sure / anywhere in South Africa |
| `industry`, `team`, `plan` | `#f-industry`, `#f-team`, `#f-plan` | pre-filled by the Plan Finder |
| `help` | `#f-help` | 7 options; pre-filled by the Solutions tags |
| `route_to` | computed | the inbox this enquiry belongs to (see routing) |
| `page` | computed | the URL the form was sent from |
| `_subject` | computed | `Quote request – <Branch or "No branch"> – <Company or name>` |
| `_gotcha` | hidden | spam honeypot; leave empty |

The page shows its own thank-you message, so there's no redirect. On success it also fires `gtag('event','generate_lead')` and a `dataLayer` `quote_submit` event, if analytics is added later.

### Branch routing

**Rule:** if a specific branch is selected, the enquiry goes to that branch's inbox. If the field is "Not sure" or blank, it goes to enquiries@seartec.co.za. *The client still needs to confirm this rule.*

| Branch | Inbox |
|---|---|
| Cape Town | capetown@seartec.co.za |
| Paarl | winelands@seartec.co.za |
| Johannesburg | johannesburg@seartec.co.za |
| Pretoria | pretoria@seartec.co.za |
| Durban | kwa-zulunatal@seartec.co.za |
| Not sure / blank | enquiries@seartec.co.za |

The mapping lives in `FORM_CONFIG.branchInbox` in `index.html`.

**On the Formspree free plan:** the free plan includes 50 submissions/month and 2 linked emails, with no multiple recipients and no routing rules. So **every submission arrives at enquiries@**. The branch is shown in the subject line, and the target inbox is in the `route_to` field. To finish the routing, set up mailbox rules on enquiries@, for example in Outlook / Microsoft 365 or Google Workspace:

- Subject contains `– Cape Town –` → forward to capetown@seartec.co.za
- Subject contains `– Paarl –` → forward to winelands@seartec.co.za
- …and so on for each branch. "No branch" stays in enquiries@.

**If the plan is upgraded later:**

- *Business plan*: use Formspree's Rules Engine to route on the `branch` (or `route_to`) field. No code change needed.
- *Or one form per branch*: needs enough linked emails. Change `FORM_CONFIG` to map each branch to its own endpoint, and pick the endpoint by branch in the submit handler.

Watch the 50 submissions/month limit. Past it, Formspree stops delivering until the next month.

### Help topic (`#f-help`)

The Solutions section's tags (`.sol-tag[data-help]`) set `#f-help` and scroll to the form. Nothing is sent until the user submits. If help-topic routing is ever added, it must cover all 7 options, including **Print management and reporting software**.

## Editing images

Images are mapped by key in `window.SEARTEC_ASSETS` near the end of `index.html`, and set on elements with `data-asset="key"` or used by the Plan Finder and Expert strips. To swap an image, replace the file in `img/` with the same name, or update the path in that map.
