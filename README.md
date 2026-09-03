## CaspaHub — public releases

This repo exists for one reason: **`ken-muritu/caspahub-flutter` (the real source) is
private, and GitHub Actions minutes on this account are currently blocked** ("recent
account payments have failed or your spending limit needs to be increased" — see
`github.com/settings/billing`). GitHub Actions is free/unlimited on *public* repos
regardless of that block, so builds run here instead, pulling the source from the private
repo via a scoped, read-only deploy token.

- **Source code**: `github.com/ken-muritu/caspahub-flutter` (private) — that's where all
  real development happens.
- **This repo**: CI config + published `.apk` releases only, nothing else. No app source
  is duplicated here beyond what a checkout step pulls in transiently during a build.
- **Once the billing block on the main account clears**, this repo becomes unnecessary —
  builds/releases can move back to `caspahub-flutter` directly. Nothing here needs
  cleaning up urgently if that happens; it can just stop being used.

## How builds happen

Two workflows:

- **`release.yml`** — `workflow_dispatch` only. Checks out `caspahub-flutter` at a given
  `ref`, builds split-per-ABI release APKs, and publishes them as a GitHub Release here
  under a given `release_tag`. Run manually (`gh workflow run release.yml -f ref=main -f
  release_tag=vX.Y.Z`) for anything you want a clean semantic version for.
- **`poll.yml`** — runs hourly (and on demand). Because `caspahub-flutter`'s own
  push-triggered CI can't run at all right now (same billing block), a push there doesn't
  auto-build anything on its own. This workflow instead checks `caspahub-flutter`'s latest
  commit on `main` against `.state/last-built-sha.txt` in this repo; if it's new, it
  records the new SHA and fires `release.yml` for you, tagged `auto-<short-sha>`. So a
  regular push to `caspahub-flutter@main` does eventually get built and released here —
  within an hour, not immediately, and under an `auto-` tag rather than a semantic version.

Both `v*` (manual, semantic) and `auto-*` (polled) tags can coexist in
[Releases](../../releases) — cut a real `vX.Y.Z` release yourself for anything worth a
proper version number; the `auto-*` tags exist so nothing silently goes unbuilt between
those.

**This does not apply to `caspahub` or `caspahub-booking`** (the Next.js web apps) — those
deploy separately via Vercel and have no relationship to this repo or to the Flutter app's
build/release cycle at all.
