# CLAUDE.md — Mijote Discourse Theme

## Project

Custom Discourse theme for the Mijote B2B vegetarian recipe community. Hosted at `discourse.norbz.tech`. Source: `github.com/Norbz/capveggie-discourse-theme`.

## Discourse Theme Structure

Only the following files are recognized by Discourse (others are ignored):

```
about.json                  ← required, theme metadata + color scheme
common/common.scss          ← main stylesheet (loaded on all devices)
common/embedded.scss        ← iframe embed only (/embed/comments)
common/head_tag.html        ← injected in <head> (fonts, etc.)
common/body_tag.html
common/header.html
common/after_header.html
common/footer.html
desktop/desktop.scss
desktop/head_tag.html
mobile/mobile.scss
mobile/head_tag.html
locales/en.yml
settings.yml
assets/
javascripts/
stylesheets/          ← arbitrary partials, usable via @import from common.scss
```

**Critical**: Discourse compiles `common/common.scss` in isolation. `@import` of local partials only works if the partial is under `stylesheets/`. Files named `common/_*.scss` are NOT auto-imported — they must be explicitly imported or their content inlined.

Refs:
- Theme structure: https://meta.discourse.org/t/developing-discourse-themes-theme-components/93648
- about.json spec: https://meta.discourse.org/t/adding-metadata-and-screenshots-to-a-theme/119205
- Theme settings: https://meta.discourse.org/t/add-settings-to-your-discourse-theme/82557

## about.json Rules

- `color_schemes` must be an **object** `{}`, not an array `[]`
- Only standard Discourse color keys are allowed (no `learn_more`, `modifiers`, etc.)
- Color values are hex strings without `#`

## CSS / SCSS Rules

- Use `rgb()` not `rgba()` — stylelint enforces `color-function-alias-notation`
- Use short hex: `#fff` not `#ffffff`
- No more than 1 declaration per single-line rule block
- Font family keywords must be lowercase: `georgia` not `Georgia`, `blinkmacsystemfont` not `BlinkMacSystemFont`

## Linting

```bash
pnpm install
pnpm lint          # runs css + js + prettier + types
pnpm lint:fix      # auto-fixes what it can
```

CI runs `pnpm lint` — it must pass before pushing.

## Git

- `git commit --amend + git push --force` to keep history clean (no fix-on-fix commits)
- Never add `Co-Authored-By` in commit messages
