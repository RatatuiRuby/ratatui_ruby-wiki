<!--
  SPDX-FileCopyrightText: 2026 Kerrick Long <me@kerricklong.com>
  SPDX-License-Identifier: CC-BY-SA-4.0
-->
# Todos (Issue Tracker)

[← Back to Wiki Index](./index.md)

Issue trackers drift into chaos. Status becomes vague. Bugs and features blur together. Urgent patches vanish.

A semantic label system organizes the work. Labels define state, type, and urgency. This system highlights priorities.

## The Color System

Label **Background Color** denotes the category. The text provides specific meaning.

* **Indigo:** State. The lifecycle of a todo.
* **Azure:** Type. The nature of the work.
* **Fuchsia:** Urgency. The priority level.
* **Lime:** Meta. Community signals.

## 1. State (Indigo)

Todos progress through a defined workflow. These labels track status and obstacles.

* **needs_info** (Mustard Text)
    * Blocking. Missing logs, reproduction steps, or clarification.
* **deferred** (Indigo Text)
    * Accepted work. Queued for later. Remains in storage until current tasks finish.
* **blocked** (Red Text)
    * Halted. Upstream bugs, missing hardware, or dependency updates prevent progress.
* **has_patch** (Teal Text)
    * **Critical.** A solution exists on the mailing list. Review the patch before writing new code.

## 2. Type (Azure)

Quick identification of work categories. These labels distinguish broken code from enhancements.

* **bug** (Red Text)
    * Broken logic. Crashes. Unexpected behavior.
* **feature** (Azure Text)
    * New functionality. Enhancements beyond current capabilities.
* **docs** (Teal Text)
    * Documentation only. Changes to wikis, READMEs, or comments. No logic changes.
* **refactor** (Violet Text)
    * Maintenance. Cleanup. Technical debt. Preserves existing behavior.

## 3. Urgency (Fuchsia)

Priority signaling. These labels identify time-sensitive tasks. Default is "Normal Priority."

* **urgent** (Red Text)
    * Immediate attention. Security vulnerabilities. Build breakage. Release blockers.
* **wishlist** (Violet Text)
    * Low priority. Potential future additions. No immediate commitment.

## 4. Meta (Lime)

Community communication and todo termination.

* **help_wanted** (Teal Text)
    * Open for contribution. Fixes or knowledge outside the maintainer's immediate capacity.
* **invalid** (Rose Text)
    * Termination state. Duplicate. Wontfix. Cannot reproduce.

### Resolution Status

Work ends. We close the todo. We assign a resolution to explain why.

* **FIXED**
    * Success. We did the work. The code is in the `trunk` branch.
* **IMPLEMENTED**
    * Success. The feature exists. It might have been there all along, or we added it recently.
* **WONT_FIX**
    * We decided not to do this. It does not fit the project goals. It introduces too much complexity.
* **BY_DESIGN**
    * Not a bug. The library behaves this way on purpose. We will not change it.
* **NOT_OUR_BUG**
    * The error is upstream. It is in your configuration, your environment, or a dependency.
* **DUPLICATE**
    * This issue exists elsewhere. Look for the link to the original todo.
* **INVALID**
    * The report is incomplete. We cannot reproduce it. The premise is flawed.
