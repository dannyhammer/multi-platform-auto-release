# Automated Release Pipeline for Rust

This repository contains a minimal example of how to set up a release pipeline for a Rust project.

## Overview

We leverage two existing GitHub Actions: [`release-please`](https://github.com/googleapis/release-please-action) and [`rust-build.action`](https://github.com/rust-build/rust-build.action).
These Actions allow us to generate a release PR and compile and upload our binary for multiple platforms in the [Releases](https://github.com/dannyhammer/multi-platform-auto-release/releases) page.

Note that you must specify a [GitHub token](https://github.com/googleapis/release-please-action?tab=readme-ov-file#github-credentials) in the [`release-please` Action](https://github.com/dannyhammer/multi-platform-auto-release/blob/main/.github/workflows/release-please.yml#L23).

The pipeline operates as follows:

1. When a push is made to `main` (either forcefully or by merging a PR), the [`release-please`](https://github.com/dannyhammer/multi-platform-auto-release/blob/main/.github/workflows/release-please.yml) action will create a [Release PR](https://github.com/googleapis/release-please-action?tab=readme-ov-file#whats-a-release-pr).
2. When the Release PR is merged, a new release will appear in the [Releases](https://github.com/dannyhammer/multi-platform-auto-release/releases) page containing the archived source code.
3. At the same time, the [`rust-build.action`](https://github.com/dannyhammer/multi-platform-auto-release/blob/main/.github/workflows/compile-and-release.yml) action is triggered to compile the binary for the [specified platforms](https://github.com/dannyhammer/multi-platform-auto-release/blob/main/.github/workflows/compile-and-release.yml#L17).
4. After all [Workflows](https://github.com/dannyhammer/multi-platform-auto-release/actions) have finished executing, the latest release on the [Releases](https://github.com/dannyhammer/multi-platform-auto-release/releases/) page will contain all compiled binaries, checksums, and source code for that release.

## Customizing

Quick tips for customization:

-   In [`compile-and-release.yml`](https://github.com/dannyhammer/multi-platform-auto-release/blob/main/.github/workflows/compile-and-release.yml):
    -   You can add/remove target platforms under the `include` section.
    -   You can change the archive type for each platform by specifying a value (or values) for `archive`.
    -   You can include additional files alongside the executable by adding them to the `EXTRA_FILES` section.

For full customization options, see the documentation for [`release-please`](https://github.com/googleapis/release-please-action) and [`rust-build.action`](https://github.com/rust-build/rust-build.action).
