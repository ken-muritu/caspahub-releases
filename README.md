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

See the [Releases](../../releases) page for APKs.
