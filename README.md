# tim-oetjen-website

[![Netlify Status](https://api.netlify.com/api/v1/badges/8ea19e82-36b5-4b19-acdc-2e599c22c9cc/deploy-status)](https://app.netlify.com/projects/timoetjen/deploys)

Source code for Tim Oetjen's website.

## How the project works

Your website is built with a tool called **Astro**. Instead of a content management system (like WordPress), the content lives directly in text files inside the `src/` folder. When a change is saved and pushed to GitHub, **Netlify** automatically rebuilds and publishes the updated site within a minute or two.

You do not need to touch the build process itself — just edit the right file, and publishing happens automatically.

To make changes you will need two tools:

- **VS Code** — a free code editor for opening and editing project files. Download at [code.visualstudio.com](https://code.visualstudio.com/).
- **GitHub Desktop** — for saving and publishing changes. Download at [desktop.github.com](https://desktop.github.com/).

Both should already be installed if you have previously worked on this project.

## Folder structure at a glance

```text
astro.config.mjs         ← Site domain (one line only)
netlify.toml             ← Hosting and redirect configuration
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
3. In **Finder**, open the `src/assets/images/` folder inside your project folder and drag the new photo there, clicking **Replace** when prompted.

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

### 6. Domain configuration

Your site's domain setup has two layers that must always stay in sync:

- **Netlify** (via its web interface) — registers each domain so that Netlify's infrastructure accepts traffic arriving on it and provisions an SSL certificate.
- **[netlify.toml](netlify.toml)** — tells Netlify what to *do* with that traffic: redirect it to your primary domain instead of serving the site directly.

If only one layer is updated, things break silently: Netlify either rejects traffic from an unknown domain, or accepts it and serves duplicate content instead of redirecting.

#### Changing the primary domain

If you move your site to a new primary domain (e.g. from `timoetjen.eu` to `timoetjen.com`):

1. **Netlify web interface** — Go to [Site configuration → Domain management](https://app.netlify.com/projects/timoetjen/domain-management) and set the new domain as the primary custom domain.

2. **GitHub Copilot** — Open [.github/prompts/change-primary-domain.prompt.md](.github/prompts/change-primary-domain.prompt.md) in VS Code and click **Run Prompt**. Copilot will ask you for the new domain, update [astro.config.mjs](astro.config.mjs) and [netlify.toml](netlify.toml) automatically, and remind you of the remaining steps.

   It will not work in browser-based tools such as Copilot web, Gemini, or ChatGPT, as those cannot access your local project files.

3. **[src/consts.ts](src/consts.ts)** — Update `CONTACT_EMAIL` if the email address is also changing.

4. Commit and push.

#### Adding or removing an alias domain

Alias domains are alternative addresses (e.g. `traductorautonomo.com`) that redirect visitors to the primary domain.

To **add** an alias:

1. Add the domain in the Netlify web interface under Domain management → Add domain alias.
2. Open [netlify.toml](netlify.toml) in VS Code, copy any one of the existing `[[redirects]]` blocks at the bottom of the file, paste it at the very end, and update the `from` line to use the new alias domain.

To **remove** an alias:

1. Remove the domain in the Netlify web interface.
2. Open [netlify.toml](netlify.toml) in VS Code and delete the five-line block for that domain: everything from `[[redirects]]` through `force = true`.

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

1. Open the relevant file in **VS Code**, make your edit, and save with **Cmd+S**.
2. Open **GitHub Desktop**. Your change appears automatically. Write a short commit message, click **Commit to main**, then click **Push origin**.
3. Netlify detects the push automatically and rebuilds the site.
4. Your live site reflects the change within about a minute.

If you are unsure whether a change is safe, or something breaks after an edit, contact the developer to review it before pushing.
