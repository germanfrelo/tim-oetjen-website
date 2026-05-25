---
agent: agent
description: Change the website's primary domain in astro.config.mjs and netlify.toml
---

Before making any changes, ask the user: "What is the new primary domain?" Then read the current files to determine the existing primary domain and apply the following changes using the domain the user provided:

**`astro.config.mjs`**

Update the `site:` property URL to `https://[NEW_DOMAIN]`.

**`netlify.toml`**

1. Update every `to = "https://.../:splat"` value to `https://[NEW_DOMAIN]/:splat`.
2. If a `[[redirects]]` block exists with `from = "https://[NEW_DOMAIN]/*"`, delete the entire block — the new primary domain must not redirect to itself.
3. Add a new `[[redirects]]` block at the end of the file for the current primary domain (it is now an alias and must redirect to the new primary). Use the same five-line format as the other redirect blocks.

Do not modify any other `from` values. Do not touch any other files.

After applying the changes, show a summary of what was updated, then remind the user of the remaining manual steps:

1. Update `CONTACT_EMAIL` in `src/consts.ts` if their email address is also changing.
2. Commit and push using GitHub Desktop: write a short commit message, click **Commit to main**, then click **Push origin**.
