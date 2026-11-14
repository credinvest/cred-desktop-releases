# CRED Desktop Releases

Download the latest CRED Desktop app for macOS:

**[Download CRED-universal.dmg](https://github.com/credinvest/cred-desktop-releases/releases/latest/download/CRED-universal.dmg)**

## What this repository is

This public repository only hosts release artifacts for CRED Desktop. It has no source code.

The source is in [credinvest/cred-platform-ts](https://github.com/credinvest/cred-platform-ts) under `apps/desktop` (private).

## Auto-update feed: do not archive or make private

Installed copies of CRED Desktop use this repository as their **auto-update feed**:

- `apps/desktop/electron-builder.yml` in cred-platform-ts publishes with `provider: github`, `owner: credinvest`, `repo: cred-desktop-releases`.
- On startup the app calls `setupAutoUpdater` (`apps/desktop/src/main/updater.ts`, which uses `electron-updater`). It reads `latest-mac.yml` from the **latest** release here. If that file lists a newer version, the app offers to download the `CRED-universal.zip` update.

Installed apps check for updates without logging in, so this repository must stay **public**. If you archive it, the next publish fails. If you delete or rename the latest release, or remove its `latest-mac.yml`, installed apps stop getting updates.

## Publishing a release

Releases are built and published from cred-platform-ts. Nothing is committed here.

1. Bump `version` in `apps/desktop/package.json` in cred-platform-ts.
2. From `apps/desktop`, build and publish with a `GH_TOKEN` that has write access to this repository's releases (contents: write on `credinvest/cred-desktop-releases`). For example:

   ```bash
   cd apps/desktop
   GH_TOKEN=<token with write access to cred-desktop-releases> npx electron-builder --mac --publish always
   ```

   Signing and notarization (`afterSign: scripts/notarize.cjs`) need the Apple credentials that script expects.
3. electron-builder uploads the following files to a release here: `CRED-universal.dmg`, `CRED-universal.zip`, their `.blockmap` files, and `latest-mac.yml`. If it creates the release as a draft, publish it so it becomes the **latest** release. Installed apps and the download link above only see the latest published release.

The download link above always points to `releases/latest/download/CRED-universal.dmg`, so you never need to edit this README for a new version.
