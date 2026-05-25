---
agent: agent
description: Change the website's primary domain in astro.config.mjs and netlify.toml
---

<!-- markdownlint-disable MD041 -->

Before making any changes, ask the user: "What is the new primary domain?" If the response is empty or does not match a valid domain pattern (letters, digits, dots, and hyphens, with at least one dot), ask again and do not proceed. Normalize the input by stripping any leading protocol (`http://` or `https://`) and any trailing slashes.

If either `astro.config.mjs` or `netlify.toml` is missing, or the `site:` property cannot be found in `astro.config.mjs`, stop and report the problem to the user without making any changes.

The existing primary domain is the host in the `site:` property of `astro.config.mjs`. If the new domain matches the existing primary domain, inform the user that no changes are needed and stop.

Apply the following changes in this exact order:

## Step 1 — Record the current primary domain

Note the host from the `site:` property in `astro.config.mjs`. You will need it in step 5.

## Step 2 — `netlify.toml`: delete the would-be self-redirect block (if it exists)

If a `[[redirects]]` block exists whose `from` value is `https://[NEW_DOMAIN]/*`, delete that entire block. (Such a block exists when the new primary was previously an alias. After Step 3 rewrites its `to` value, it would redirect `[NEW_DOMAIN]` to itself.)

## Step 3 — `netlify.toml`: update all `to` values

For every remaining `[[redirects]]` block, update the `to` value only if its host exactly matches the current primary domain recorded in Step 1. Replace that host with `[NEW_DOMAIN]` and preserve the path (typically `/:splat`). Leave all other `to` values and all `from` values unchanged.

## Step 4 — `astro.config.mjs`

Update the `site:` property URL to `https://[NEW_DOMAIN]`.

## Step 5 — `netlify.toml`: add a redirect for the old primary domain

Append a new `[[redirects]]` block at the end of the file for the current primary domain recorded in step 1. Use this exact format:

```toml
[[redirects]]
from = "https://[OLD_DOMAIN]/*"
to = "https://[NEW_DOMAIN]/:splat"
status = 301
force = true
```

Do not touch any other files.

After applying all changes, show a summary of what was updated, then remind the user of the remaining manual steps:

1. Update `CONTACT_EMAIL` in `src/consts.ts` if their email address is also changing.
2. Commit and push using GitHub Desktop: write a short commit message, click **Commit to main**, then click **Push origin**.
