# diveriontech-web

The Diverion Tech website — static pages served by GitHub Pages at
**https://diveriontech.com**.

No build step, no dependencies. Each page is self-contained: all CSS inline, no
external scripts or fonts. The only assets are two local PNGs. Edit and push.

## Structure

```
index.html          Diverion Tech — Consulting-forward homepage (teal / black / white)
mickle/index.html   Mickle product page (keeps its own green identity)
images/
  logo-mark.png     eagle mark, transparent background, 320px wide
  logo-lockup.png   eagle + DIVERION TECH wordmark, transparent, 640px wide
CNAME               diveriontech.com — written by GitHub, do not hand-edit
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

Push to `main`. GitHub Pages rebuilds automatically (Settings → Pages, source
`main` / root).

The repo is deliberately a **project repo**, not `ishankerketta.github.io` — the
`mickle` DNS CNAME points at that hostname, and a user-site repo claiming a
custom domain can start redirecting it.

## Domain configuration — LIVE as of 15 Sept 2026

`diveriontech.com` and `www.diveriontech.com` both serve this repo over HTTPS.
`www` 301s to the apex via GitHub's automatic redirect. **Nothing here needs
doing — this section is a record of the working configuration.**

**Squarespace is the registrar only.** No Squarespace website plan. The
"Squarespace Defaults" preset block was deleted entirely: four parking `A @`
records, `CNAME www → ext-sq.squarespace.com`, and an `HTTPS @` record whose
`ipv4hint` still pointed at Squarespace IPs (that last one matters — browsers
honouring HTTPS resource records can be steered back to Squarespace even with
correct A records).

Current records under **Custom records**, TTL 4 hrs:

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

GitHub side: Settings → Pages → Custom domain `diveriontech.com`, Enforce HTTPS
ticked. **Setting the custom domain there is what created the `CNAME` file in
this repo — never write that file by hand.** Committing a CNAME before DNS
resolves makes Pages redirect the github.io URL to a domain that doesn't answer,
taking the site dark at both addresses.

### If this ever needs redoing

- Squarespace demands **step-up 2FA partway through a DNS editing session** and
  does not say so — the ADD RECORD button just silently stops responding. Look
  for the verification modal before assuming the UI is broken.
- The TYPE dropdown **opens upward** when the form sits low in the viewport, and
  a mis-aimed click lands on TXT. Confirm the type before saving each record.
- **Verify against a resolver, not the console.** Two A records were missed on
  the first pass and the panel looked complete. `dig @8.8.8.8 A diveriontech.com`.

## Records that must NEVER be touched

These live in **Custom records** and carry email and Mickle's hosted pages.

| Record | Purpose |
|---|---|
| `CNAME mickle → ishankerketta.github.io` | Mickle legal + auth confirm pages (`mickle-web` repo) |
| `MX @ → smtp.google.com` | Google Workspace mail |
| `MX send → feedback-smtp.ap-northeast-1.amazonses.com` | Resend bounce handling |
| `TXT @ → v=spf1 include:_spf.google.com ~all` | SPF (Workspace) |
| `TXT send → v=spf1 include:amazonses.com ~all` | SPF (Resend subdomain) |
| `TXT google._domainkey` | Google DKIM |
| `TXT resend._domainkey` | Resend DKIM |
| `TXT _dmarc` | DMARC policy |

A broken SPF or DKIM record produces no error — Mickle's password-reset and
signup-confirmation emails simply start landing in spam.

## Open items

- [ ] **Gmail send-as for `consulting@`.** The alias exists and receives, but an
      alias does not send. Without Gmail → Settings → Accounts → "Send mail as",
      every reply to a client leaves from `ceo@diveriontech.com` — the recovery
      address for Play Console, Supabase, Resend and the registrar, deliberately
      kept off every public page. Test: mail the alias from outside, reply, read
      the From: line.
- [ ] **Screen the eagle mark.** Generated with ChatGPT. Trademark rights come
      from use in commerce so filing is unaffected, but copyright exclusivity
      over AI-generated artwork is doubtful and eagle-plus-monogram is a crowded
      genre. Run it through the WIPO Global Brand Database, as the name was,
      before any signage or print spend.
- [ ] **Play Store link.** Mickle reads "Coming soon" in two places
      (`mickle/index.html` hero and the homepage Mickle section). Replace both
      with the real listing URL once production access is granted — earliest
      25 Sept 2026, and only after the 14-day continuous-tester clock completes.
- [ ] **`_dmarc` publishes `ceo@diveriontech.com`** in its `rua=` field, which is
      publicly queryable. Point it at a role address. Safe to do now — no Play
      application is pending.
- [ ] **The Assam dashboard stays described as method, not product** until it is
      actually built and usable with clients.

## Images

The two logo PNGs were derived from the original ChatGPT render by keying the
black background to transparency and splitting the eagle from the lockup. They
sit on any dark ground without a visible box.

Commit only web-sized, compressed files (Squoosh or TinyPNG first). GitHub Pages
serves whatever is in the repo, unoptimised, to every visitor. Keep source and
full-resolution files out — history is public and permanent.
