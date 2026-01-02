<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->
Subject: [ANN] Adopting Trunk-Based Development

The project is changing its branching strategy to better support both
users and contributors.

## For Users:

The default branch of the repository is now `stable`. It points to the
latest release. This means the README and examples you see on the web
will always match the gem you install.

## For Contributors and Cutting Edge Users:

`main` has been renamed to `trunk`. This is where active development
happens. If you want to contribute or use the absolute latest features:

    git checkout trunk

All pull requests should target `trunk`.

--

Thanks,
Kerrick Long
