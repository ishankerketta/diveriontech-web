# diveriontech-web

The Diverion Tech website — static pages served by GitHub Pages.

No build step, no dependencies. Each page is self-contained: all CSS inline, no
external scripts or fonts. The only assets are two local PNGs. Edit and push.

## Structure

```
index.html          Diverion Tech — Consulting-forward homepage (teal / black / white)
mickle/index.html   Mickle product page (keeps its own green identity)
images/
  logo-mark.png     eagle mark, transparent background, 320px wide
  logo-lockup.png   eagle + DIVERION TECH wordmark, transparent, 640px wide
CNAME               custom domain for GitHub Pages
```

## Navigation

Both pages share one top-level tab bar: **Consulting · Method · Mickle · Contact**.
Mickle is a real destination (`mickle/`), not a page anchor, and is styled as a
green pill so it reads as its own place rather than another scroll target — filled
and marked `aria-current="page"` when you are on it. The homepage also carries a
full Mickle section above the footer, and the Mickle page keeps a teal "A product
of Diverion Tech" strip above its header. Adding a third venture later means one
more tab plus one more folder.

**The two pages look different on purpose.** Diverion Tech is the parent brand
(fluorescent teal on black, eagle mark). Mickle is a product brand with its own
dark-green identity, carried over from the app and its email templates. Do not
"unify" them — a parent entity and a product are allowed to look distinct, and
Mickle's green is already in the app, the printed report and four Supabase email
templates.

## Voice

Copy is written in **neutral voice — no first-person, no corporate "we"**.
"Diverion Consulting works with…", never "we work with…" or "I work with…".
This was a deliberate choice: it reads professional without claiming headcount
that doesn't exist, and it doesn't need rewriting if someone is hired.

Two related constraints, both load-bearing:

- Every page states that **Diverion Tech is a trading name of Ishan Savio
  Kerketta, not a registered company**. This matches the Mickle Terms. Do not
  quietly drop it.
- The homepage footer adds that consulting engagements **do not constitute
  financial, investment, legal or tax advice**. Keep it — the consulting is
  data-led business advisory, and the distinction matters for a sole operator
  with no professional indemnity cover.

## Email addresses per page — keep these separate

`consulting@` is the **only** address on the homepage. `support@`, `grievance@`
and `security@` are Mickle's addresses and appear on `mickle/index.html` only —
they are referenced in the app, its Play listing and its legal pages, and listing
them under Diverion Consulting mixes up two different businesses' inboxes.

The homepage instead carries one line routing app users to the Mickle page. If a
future venture needs its own address, give it its own page rather than adding a
row to the consulting contact block.

## Deploying

1. Push to GitHub as a **project repo** — e.g. `diveriontech-web`.
   Do **not** name it `ishankerketta.github.io`; the `mickle` DNS CNAME points at
   that hostname, and a user-site repo claiming a custom domain can start
   redirecting it.
2. Settings → Pages → Source: `main` branch, `/ (root)`.
3. Live at `https://ishankerketta.github.io/diveriontech-web/`. Verify there first.

## Pointing diveriontech.com at it — LATER

**Do not do this while Google Play production access is under review.** Nothing
here is needed for that review.

When ready, in Squarespace → Domains → diveriontech.com → DNS Settings:

**Delete the entire "Squarespace Defaults" block.** That removes the four parking
`A @` records, the `www` CNAME to `ext-sq.squarespace.com`, and the `HTTPS @`
record. All three are Squarespace website records; nothing in Mickle uses them.

> The `HTTPS @` record matters. Its `ipv4hint` still lists Squarespace IPs, so
> browsers honouring HTTPS resource records can be steered back to Squarespace
> even after the A records are correct. It must go.

Then add, under **Custom records**:

| Type | Name | Data |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| AAAA | @ | 2606:50c0:8000::153 |
| AAAA | @ | 2606:50c0:8001::153 |
| AAAA | @ | 2606:50c0:8002::153 |
| AAAA | @ | 2606:50c0:8003::153 |
| CNAME | www | ishankerketta.github.io |

Finally: Settings → Pages → Custom domain → `diveriontech.com`, wait for the DNS
check to pass, then tick **Enforce HTTPS**.

## Records that must NEVER be touched

These live in **Custom records** and carry email and Mickle's hosted pages.

| Record | Purpose |
|---|---|
| `CNAME mickle → ishankerketta.github.io` | Mickle legal + auth confirm pages |
| `MX @ → smtp.google.com` | Google Workspace mail |
| `MX send → feedback-smtp.ap-northeast-1.amazonses.com` | Resend bounce handling |
| `TXT @ → v=spf1 include:_spf.google.com ~all` | SPF (Workspace) |
| `TXT send → v=spf1 include:amazonses.com ~all` | SPF (Resend subdomain) |
| `TXT google._domainkey` | Google DKIM |
| `TXT resend._domainkey` | Resend DKIM |
| `TXT _dmarc` | DMARC policy |

A broken SPF or DKIM record produces no error — Mickle's password-reset and
signup-confirmation emails simply start landing in spam.

## Before this goes live

- [ ] **Create `consulting@diveriontech.com`.** Make it an **alias → inbox**, not
      a Google Group. Client correspondence gets replied to personally; the Group
      pattern (`support@`, `grievance@`) is for open-to-receive, closed-to-read.
      **Configure Gmail send-as for it** — otherwise replies leave from `ceo@`
      and re-expose the address that was deliberately scrubbed from the legal
      pages. The site links `consulting@` in two places and it must not bounce.
- [ ] **Screen the eagle mark.** It was generated with ChatGPT. Trademark rights
      come from use in commerce, so filing is unaffected — but copyright
      exclusivity over AI-generated artwork is doubtful, and eagle-plus-monogram
      is a crowded genre. Run it through the WIPO Global Brand Database, as the
      name was, before any signage or print spend.
- [ ] **Play Store link.** The Mickle page reads "Coming soon" and collects
      interest by email. Replace with the real listing URL once production access
      is granted, in both `mickle/index.html` and the homepage venture card.
- [ ] **The Assam dashboard is described as method, not product.** Keep it that
      way until it is actually built and usable with clients.

## Later

- `_dmarc` publishes `ceo@diveriontech.com` in its `rua=` field, which is
  publicly queryable. Point it at a role address — but not during the Play review.
- If the homepage outgrows itself, split Consulting onto `/consulting/` and leave
  the homepage as a short parent landing.

## Images

The two logo PNGs were derived from the original ChatGPT render by keying the
black background to transparency and splitting the eagle from the lockup. They
sit on any dark ground without a visible box.

Commit only web-sized, compressed files (Squoosh or TinyPNG first). GitHub Pages
serves whatever is in the repo, unoptimised, to every visitor. Keep source and
full-resolution files out — history is public and permanent.
