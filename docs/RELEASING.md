# Releasing

How a release of the Tellybox add-on is made. It mirrors [Tellybox's own process](https://github.com/sandermvanvliet/tellybox/blob/main/docs/RELEASING.md): a release only happens when the owner pushes a `v*` tag.

## Versioning

[Semantic versioning](https://semver.org/): `MAJOR.MINOR.PATCH`. The single source of the version is `version` in `tellybox/config.yaml`. Home Assistant offers an update when that value changes on `main`, so merging a version bump is what ships an update to users; the tag and GitHub release record it.

The add-on has no image tag of its own: Home Assistant pulls `ghcr.io/sandermvanvliet/tellybox:<version>`. **The add-on version must therefore be a released Tellybox version**, so release Tellybox first (its `docs/RELEASING.md`) and use the same number here. An add-on-only fix (options, docs) cannot get its own version unless Tellybox releases that version too.

## Before you start

- `main` is green (the *Lint* workflow passed).
- The Tellybox image `ghcr.io/sandermvanvliet/tellybox:X.Y.Z` exists.
- `tellybox/CHANGELOG.md` describes everything since the last release under `## Unreleased`, including anything a user must do when upgrading.

## Steps

1. On a branch, set `version` in `tellybox/config.yaml` to `X.Y.Z` and rename `## Unreleased` in `tellybox/CHANGELOG.md` to `## X.Y.Z`. Merge it by pull request.
2. Update your checkout to the merge commit on `main`.
3. Tag that commit and push the tag:

   ```sh
   git checkout main && git pull
   git tag -a vX.Y.Z -m "Tellybox add-on X.Y.Z"
   git push origin vX.Y.Z
   ```

The tag must be `v` plus the exact `version` from `tellybox/config.yaml`.

## What CI does

Pushing a `v*` tag runs `.github/workflows/release.yaml`, which fails before publishing anything if a check fails:

1. The tag matches `version` in `tellybox/config.yaml`.
2. `tellybox/CHANGELOG.md` has a non-empty `## X.Y.Z` section.
3. The image `ghcr.io/sandermvanvliet/tellybox:X.Y.Z` exists.
4. The add-on linter passes.
5. The GitHub release is created (only if it doesn't exist yet) with that changelog section as its notes.

## Check the result

- The workflow run for the tag is green in the Actions tab.
- `gh release view vX.Y.Z` shows the release and the changelog notes.
- In Home Assistant, **Settings > Add-ons > Add-on store**, menu > **Check for updates** shows the new version.

## If a release is bad

Never move or delete a tag that people may have pulled. Fix the problem on `main` and release the next patch version. Delete a release only if it was never used, for example when the workflow failed halfway, and then delete its tag too so the version can be tagged again.
