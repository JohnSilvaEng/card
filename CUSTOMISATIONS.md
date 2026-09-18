# What is mine, what is upstream

LibCard vendored at **v0.2.0-39-g7ed7bba** (crs48/LIBCard, MIT).
The clone's remote is named `upstream`, not `origin`, so nothing can be pushed
to the upstream project by accident.

Everything below is deliberately small: the whole point of LibCard is that one
config file and one theme file carry the customisation, so `pnpm run update`
can replace the engine underneath without conflicts.

## Mine — edit these

| File | What it is |
|---|---|
| `libcard.config.yaml` | All content: profile, contact, socials, links, card mode, base path. `update.mjs` never touches it. |
| `themes/silva.yaml` | Custom theme matching johnsilvaeng.github.io. Themes you author yourself are preserved by `update.mjs`. |
| `public/john-silva.jpg` | Headshot, copied from the portfolio. `public/` is never touched by `update.mjs`. |
| `mise.toml` | Pins Node 22 + pnpm 10.11.1 for this directory only. Not an upstream file. |
| `CUSTOMISATIONS.md` | This file. |

## Deleted from upstream

`public/avatar.jpg`, `public/avatar.svg` — the upstream author's sample avatar.

## Generated — never edit by hand

`src/data/themes.json`, `src/styles/themes.gen.css`, `themes/theme.schema.json`,
`libcard.schema.json`, `themes/README.md` (its theme table).

These show as modified in `git status` only because adding `themes/silva.yaml`
regenerates them. `prebuild` rebuilds them on every build and `update.mjs`
explicitly skips them.

## Everything else

Untouched upstream: `src/**`, `scripts/**`, `astro.config.mjs`, `package.json`,
`docs/**`. No component was rewritten or replaced.

## Updating LibCard later

```bash
pnpm run update      # replaces the engine, keeps config / public / your themes
pnpm run build
```

`docs/UPGRADING.md` is the upstream guide.

## Known deviation — typography

The portfolio uses Space Grotesk, Inter and JetBrains Mono from Google Fonts.
LibCard restricts `tokens.font` to four network-free system stacks by design,
so a webfont cannot be set through the supported theming mechanism. Overriding
it would add a third-party request on every visit, which the brief rules out.
The card therefore matches the portfolio on colour, spacing, shape and
hierarchy, but uses the system sans stack for letterforms.

## Local preview

```bash
cd /home/john/Work/card
pnpm run dev        # http://localhost:4321/card  — live reload
# or, to check exactly what ships:
pnpm run build && pnpm run preview
```

Note `/card` in the URL: `site.base` is `/card`, so the dev and preview servers
mount the site there too.
