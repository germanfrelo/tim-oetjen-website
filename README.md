# tim-oetjen-website

[![Netlify Status](https://api.netlify.com/api/v1/badges/8ea19e82-36b5-4b19-acdc-2e599c22c9cc/deploy-status)](https://app.netlify.com/projects/timoetjen/deploys)

Source code for Tim Oetjen's website.

## How the project works

Your website is built with a tool called **Astro**. Instead of a content management system (like WordPress), the content lives directly in text files inside the `src/` folder. When a change is saved and pushed to GitHub, **Netlify** automatically rebuilds and publishes the updated site within a minute or two.

You do not need to touch the build process itself — just edit the right file, and publishing happens automatically.

## Folder structure at a glance

```text
astro.config.mjs         ← Site domain (one line only)
netlify.toml             ← Hosting redirect rules (two lines only)
src/
  consts.ts              ← Global site data (title, description, email)
  pages/
    index.astro          ← All visible content on the page
  components/
    Footer.astro         ← Footer text
  assets/
    images/
      photo-profile.webp ← Your profile photo
  styles/
    global/
      global.css         ← Colours, fonts, and spacing
```

## What you can safely edit

### 1. Global site data

**File:** [src/consts.ts](src/consts.ts)

This file contains named values that are reused across the whole site.

Open the file and edit only the text between the double quotes on each line. Do not remove the quotes or the semicolons at the end of each line.

### 2. Page content (all visible text)

**File:** [src/pages/index.astro](src/pages/index.astro)

This is the single page of your site. Everything visible — the hero introduction, the lists of project types, language combinations, services, testimonials, and contact buttons — lives in this file.

The file mixes HTML-like tags with your text. Editable content sits between opening and closing tags.

**Safe to change:** any text between `>` and `<`.

**Do not change:** the tag names themselves (`h1`, `p`, `ul`, `li`, `a`, etc.), any `class` or `data-` attributes, or curly-brace expressions like `{CONTACT_EMAIL}`.

#### Updating a testimonial

Each testimonial follows this structure:

```html
<figure class="flow">
  <blockquote>
    <p>«&nbsp;The quote text here.&nbsp;»</p>
  </blockquote>
  <figcaption>
    <b>Job title or name</b>
  </figcaption>
</figure>
```

To update a quote, replace the text inside `<p>«&nbsp;…&nbsp;»</p>`. To update the attribution, replace the text inside `<b>…</b>`.

### 3. Profile photo

**File:** [src/assets/images/photo-profile.webp](src/assets/images/photo-profile.webp)

To replace your profile photo:

1. Export the new photo in **WebP format** at roughly 768 × 1024 pixels (portrait orientation).
2. Name the file exactly `photo-profile.webp`.
3. Replace the existing file at the path above.

The filename must stay exactly the same, otherwise the page will break.

### 4. Footer

**File:** [src/components/Footer.astro](src/components/Footer.astro)

The footer contains your name and professional title on the first line of text. You can update that line if needed. Do not remove the developer credit on the second line.

### 5. Colours and visual style

**File:** [src/styles/global/global.css](src/styles/global/global.css)

Open the file and find the `:root { }` block near the top. The colour properties are listed under the `Appearance` comment. Each property name describes its role:

- `--color-base` — main background colour.
- `--color-base-2` — secondary background colour.
- `--color-contrast` — primary text and button colour.
- `--color-contrast-2` — secondary/muted text colour.
- `--color-accent` — accent colour.
- `--color-accent-2` — darker accent colour.

Colours use the `hsl()` format: `hsl(hue degrees, saturation%, lightness%)`. Use any HSL colour picker online to find the values you need, then replace only the `hsl(…)` value on the corresponding line.

There is also a `@media (prefers-color-scheme: dark)` block further down with equivalent values for dark mode. If you change a light-mode colour, update the matching dark-mode colour too.

### 6. Site domain

**File:** [astro.config.mjs](astro.config.mjs#L6)

Line 6 contains the `site` property — the canonical URL of your website. If your domain changes, update only the URL value on that line. Leave every other line in this file untouched.

### 7. Redirect rules

**File:** [netlify.toml](netlify.toml#L13-L14)

Lines 13–14 contain the redirect that forwards traffic from the old Netlify subdomain to your custom domain. If your domain changes, update the `to` value on line 14 to match your new domain. Leave the `from` value and everything else in the file untouched.

## What you should not edit

The following files are technical and should not be changed without developer involvement:

- `tsconfig.json` — TypeScript configuration.
- `package.json` — Project dependencies and scripts.
- `prettier.config.js`, `stylelint.config.js` — Code formatting rules.
- `src/env.d.ts` — TypeScript type declarations.
- `src/layouts/` — Page shell templates.
- `src/components/BaseHead.astro`, `src/components/HeaderLink.astro`, `src/components/Header.astro` — Technical components.
- `src/styles/` (all files except `global/global.css`) — Layout and utility styles.
- `public/fonts/` — Font files.

## How changes are published

1. Edit the relevant file and save it.
2. Commit the change to Git and push it to the `main` branch on GitHub.
3. Netlify detects the push automatically and rebuilds the site.
4. Your live site reflects the change within about a minute.

If you are unsure whether a change is safe, or something breaks after an edit, contact the developer to review it before pushing.
