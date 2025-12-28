<!--
  SPDX-FileCopyrightText: 2025 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->
# Documentation Guide

[← Back to Wiki Index](./index.md)

This guide helps contributors understand where to document their work.

## Where things go

*   **In-Code (Ruby)**: Use RDoc (`# ...`) for public API documentation and `.rbs` files in `sig/` to define types.
    *   *Examples*: Method parameters, return types, class purposes, usage instructions, and simple code snippets.
*   **In-Code (Rust)**: Use Rustdoc (`/// ...`) for information relevant only to Rust developers.
*   **Repo Docs (`doc/`)**: Use Markdown in the main repository for information that is likely to change as the codebase changes.
    *   *Examples*: How to use the library, how to test your application, architecture and design docs.
*   **Wiki**: Use this Wiki for information about the project that is largely independent of the codebase.
    *   *Examples*: Process, roadmap, governance, real-world applications using the library, and archives of communication and decision-making. 

## Key Links
*   [Repo Documentation](https://git.sr.ht/~kerrick/ratatui_ruby/tree/HEAD/doc/index.md)
*   [Wiki Index](./index.md)
