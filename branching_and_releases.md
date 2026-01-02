<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->
# Branching and Releases

[← Back to Wiki Index](./index.md)

This project follows a trunk-based development workflow with a stable default branch.

## Branches

*   **`trunk`**: The active development branch. This is the "Workshop". All pull requests, features, and daily work happen here. The code here may be unreleased. Use this branch if you are a contributor or want the latest cutting-edge features.
*   **`stable`**: The default branch. This is the "Storefront". It always points to the latest release tag. It allows users to browse the documentation for the version they can actually install.

## Release Process

When a new version is released:
1.  The commit on `trunk` is tagged.
2.  The `stable` branch is fast-forward merged to that tag.

This ensures `stable` is always a strict subset of `trunk`'s history.

## For Contributors

Target your pull requests to **`trunk`**.

If you clone the repo, you will be on `stable` by default. You must `git checkout trunk` to begin contributing.
