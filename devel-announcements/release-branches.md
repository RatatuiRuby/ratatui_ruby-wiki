<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->
Subject: [ANN] Adopting Release Branches for 1.0

I am changing the release strategy for ratatui_ruby to
support the 1.0 series while continuing development on trunk.

## For App Developers:

The `stable` branch continues to point to the latest stable release.
The 1.0 series begins with `v1.0.0-beta.1`.

## For Contributors:

I now use **release branches** alongside trunk-based development:

- `trunk` → Active development (future features, alignment work)
- `release/1.0` → The 1.0 release series (beta, rc, and final releases)

### Branch Structure

```
trunk        ──●──●──●──●──●──●──●──●──●──  (1.2.0.dev, future work)
               \           \
release/1.0  ───●──●──●     \              (1.0.0, 1.0.1, ...)
                             \
release/1.1                   ●──●──●──    (1.1.0, 1.1.1, ...)
```

### Bugfix Version (Patch) Workflow

1. Fix the bug on `trunk` first
2. Cherry-pick to `release/1.0`:
   ```
   git checkout release/1.0
   git cherry-pick <commit>
   ```
3. Bump patch version and release from `release/1.0`

### New Version (Major & Minor) Workflow

1. When trunk is ready for release, decide the version:
   - **Breaking changes?** → bump major (e.g., `2.0.0`)
   - **New features only?** → bump minor (e.g., `1.1.0`)
2. Create a release branch:
   ```
   git checkout trunk
   git checkout -b release/1.1  # or release/2.0
   ```
3. Release from the branch (beta, rc, final as needed)
4. Bump trunk to the next dev version

### Why This Change?

This allows us to:
- Ship 1.0 releases while continuing larger alignment work on trunk
- Backport critical fixes without including unfinished features

--

Thanks,
Kerrick Long
