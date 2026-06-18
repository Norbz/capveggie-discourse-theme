# AGENTS.md — Mijote Discourse Theme

## Before You Start

Read `CLAUDE.md` for project context, file structure rules, and about.json constraints.

## Mandatory: Lint Before Every Commit

```bash
pnpm lint
```

All checks must pass (CSS, JS, prettier, types). Fix errors with `pnpm lint:fix` then verify manually for anything the autofix can't handle.

## Common Lint Pitfalls

| Error | Fix |
|---|---|
| `color-function-alias-notation` | Use `rgb(r, g, b, a)` not `rgba(r, g, b, a)` |
| `color-hex-length` | Use `#fff` not `#ffffff` |
| `declaration-block-single-line-max-declarations` | Split multi-declaration one-liners to multiple lines |
| `value-keyword-case` | Lowercase font names: `georgia`, `blinkmacsystemfont` |
| `rule-empty-line-before` | Add or remove blank lines between rules as required |

## Commit Style

- Amend the last commit rather than stacking fix commits: `git commit --amend --no-edit && git push --force`
- Never add `Co-Authored-By` to commit messages

## Testing Changes

After pushing, go to `discourse.norbz.tech/admin/customize/themes/5`, click **Vérifier les mises à jour**, then **Mettre à jour vers la dernière version** if a force-push was detected.

## SCSS Architecture

All styles live in `common/common.scss` (single file, no local `@import`). Sections are delimited by banner comments:

```
// TOKENS        — CSS custom properties (:root)
// BASE          — body, headings, links, code
// LAYOUT        — #main-outlet, .boxed.white
// NAVIGATION    — .d-header, .d-header-icons
// BUTTONS       — .btn, .btn-primary, .btn-default
// TOPIC LIST    — card rows with border-collapse: separate
// TOPIC PAGE    — post body, timeline
// TAGS          — .discourse-tag (DM Mono, uppercase)
// SIDEBAR       — .sidebar-wrapper
// CATEGORIES    — category grid
// ADMIN         — admin panel overrides
```

## Key CSS Facts

- `.boxed.white` is Discourse's admin panel wrapper — add `padding` and `border` here for card styling
- Topic list card radius requires `border-collapse: separate` + radius on `td:first-child` / `td:last-child` (CSS doesn't support `border-radius` on `<tr>`)
- `common/embedded.scss` (not `embed.scss`) is the correct filename for embed-specific styles
- Discourse CSS variables (`--primary`, `--tertiary`, etc.) are server-rendered from `color_schemes` in `about.json` — overriding them in SCSS only affects the theme bundle, not the embed bundle
