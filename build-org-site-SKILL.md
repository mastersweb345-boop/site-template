---
name: build-org-site
description: >-
  Turn a folder of a company's/NGO's photos, logos and banners into a live
  2-page website (Home + Contact) with an embedded Google Form, hosted free
  on GitHub Pages under the mastersweb345-boop account. Use when the
  user hands you a folder named after a business/NGO and wants a website,
  or says things like "build a site for this folder", "make a website for
  [company]", "do the website thing again", "new client site". The person
  asking is usually non-technical — they should never need to touch git,
  GitHub, or code themselves.
---

# Build Org Site

Repeatable recipe for turning "a folder of photos" into a live, working
2-page site. The whole point: the non-technical person hands you a folder
and answers a few plain-English questions in chat — every git/GitHub/CLI
step happens on your side, invisibly.

Reference implementation: https://github.com/mastersweb345-boop/roshan-bhavishya
Template repo (marked `is_template: true`): `mastersweb345-boop/site-template`

## 1. Mine the folder before asking anything

The folder name is the company/org name. Before asking the user a single
question, `Read` every image in it (logos, banners, event photos, flyers).
People running these orgs already have banners with the name, tagline,
services/programs, address, phone numbers, registration numbers baked in as
text — extract all of it. Re-asking for facts already visible in their own
photos wastes their time and reads as not having done the work.

Flag, don't silently use: any banner text with an inconsistent spelling of
the org name or contradictory info across images. Exclude that specific
image from the site and tell the user why in one line, rather than
publishing a typo of their own company's name.

## 2. Ask only what's still missing

Typically just:
- Confirm org type/one-line mission if it's not obvious from the banners.
- Whether a Google Form for inquiries already exists.
  - If not: walk them through creating one themselves at forms.google.com
    (you cannot log into their Google account for them). Give a short
    numbered list of fields to add (Name, Phone, Email, Message is the
    default — adjust to the business). Ask them to send back the share
    link or the `<>` embed iframe.
- Any contact detail truly not visible anywhere in the folder (e.g. email,
  if the business wants one public).

Do not fabricate taglines, addresses, phone numbers, or registration
numbers. If it's not in the photos and the user hasn't said it, ask.

## 3. Scaffold from the template

```bash
gh repo create mastersweb345-boop/<company-slug> --public \
  --template mastersweb345-boop/site-template --clone
```

`<company-slug>` = kebab-case org name (e.g. `roshan-bhavishya`). This
clones a fresh copy of `index.html` / `contact.html` / `style.css` with
`{{TOKEN}}` placeholders (see that repo's README for the full token list).

Fill in every token by hand with `Edit` using the content gathered in steps
1–2. Delete sections that don't apply (e.g. the `.pillars` block if there
aren't 3 clean focus areas). Add/remove `.card` blocks in Programs to match
however many services actually exist — don't pad to a fixed number.

Copy the client's real photos into `images/`, replacing the gallery
placeholder. Skip AI-generated mockups/collages unless the user confirms
they're meant to represent the real org (people sometimes drop AI test
images into these folders — real event photos are usually better trust
signals than generated ones).

For the Google Form: take the share link and turn it into an embed by
appending `?embedded=true` to the `viewform` URL, unless the user already
gave you a full `<iframe>` snippet.

## 4. Preview before anything goes live

Static HTML, so `python3 -m http.server` is enough — no build step, no
framework. Set up `.claude/launch.json` in the client's folder if not
already there (see the Roshan Bhavishya repo for the exact config), then
use the Browser pane to check both pages at desktop **and** mobile width.
The embedded Google Form is the thing most likely to look broken on
mobile — always check it specifically.

## 5. Git + GitHub — confirm before anything public

`git init` (if the template clone didn't already set one up cleanly),
commit, is all fine to do without asking.

**Before creating the public repo, pushing, or enabling Pages: tell the
user what you're about to make public and get an explicit yes.** This
mirrors what happened with Roshan Bhavishya — build and preview freely,
but the go-live step needs their sign-off every time, not just the first
time.

Once confirmed:

```bash
gh repo create mastersweb345-boop/<company-slug> --public --source=. --remote=origin --push
gh api repos/mastersweb345-boop/<company-slug>/pages -X POST -f "source[branch]=main" -f "source[path]=/"
```

(Skip the first line if you already created the repo from the template in
step 3 — just push to the existing `origin`.)

Verify the live URL actually loads (curl or the Browser pane) before
telling the user it's done — Pages takes ~30–60s to build the first time.

Live URL pattern: `https://mastersweb345-boop.github.io/<company-slug>/`

## 6. What to tell the user at the end

- The live URL.
- Which images (if any) you left out and why.
- That a custom domain is a separate later step (a `CNAME` file + DNS
  records pointed at GitHub Pages) — don't do it unless asked.

## Non-negotiables

- Never log into the user's own Google/GitHub account on their behalf, or
  ask them for passwords/tokens. Google Form creation and GitHub OAuth
  device-code approval are things they do themselves, in their own
  browser — you only generate the device code and hand them the URL.
- All client sites go under the single `mastersweb345-boop` GitHub
  account as separate public repos — no per-client accounts.
- Don't add a CMS, build tooling, JS framework, or contact-form backend.
  Plain HTML/CSS + an embedded Google Form is the whole product. If a
  client's needs genuinely outgrow that, flag it as a one-line exception
  rather than upgrading the template for everyone.
