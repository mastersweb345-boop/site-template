# Site Template

Generic 2-page site (Home + Contact) with an embedded Google Form. Used by the
`build-org-site` Claude skill — not meant to be filled in by hand.

Placeholder tokens to replace per client:

- `{{COMPANY_NAME}}`
- `{{TAGLINE}}`
- `{{META_DESCRIPTION}}`
- `{{PILLAR_1}}` / `{{PILLAR_2}}` / `{{PILLAR_3}}` (delete the whole `.pillars` block if not applicable)
- `{{ABOUT_TEXT}}`
- `{{PROGRAMS_HEADING}}`, `{{PROGRAM_n_TITLE}}` / `{{PROGRAM_n_DESC}}` (add/remove `.card` blocks as needed)
- `{{CTA_TEXT}}`
- `{{ADDRESS}}`
- `{{PHONE_LINKS}}` — `<a href="tel:+91...">...</a>` per number
- `{{REGISTRATION_LINE}}` / `{{OPTIONAL_REGISTRATION_LINE}}` — leave empty if not applicable
- `{{GOOGLE_FORM_EMBED_URL}}` — the form's viewform URL with `?embedded=true` appended
- `{{CONTACT_SUBTEXT}}`
- Gallery `<img>` tags in `index.html` — replace with the client's real photos in `images/`
- `images/hero.jpg` — referenced by `.hero` background in `style.css`
