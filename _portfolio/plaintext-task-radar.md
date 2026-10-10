---
title: "plaintext-task-radar: overdue, due-soon, and stale tasks from Markdown notes"
excerpt: "A small command-line tool that reads a folder of Markdown project notes, reports overdue, due-soon, and stale tasks, and lints the notes for tasks that no report would ever see."
collection: portfolio
group: "Tools and software"
order: 3
tags: [Python, CLI, Markdown, testing]
header:
  teaser: projects/plaintext-task-radar.jpg
teaser_alt: "HTML report for the demo notes, with counts of 4 overdue, 5 due soon, 1 stale, and 3 project issues, and a table of overdue tasks"
---

<a class="project-cta" href="https://github.com/Adrian-I-lab/plaintext-task-radar">View the code on GitHub</a>

![HTML report for the demo notes, with counts of 4 overdue, 5 due soon, 1 stale, and 3 project issues, and a table of overdue tasks](/images/projects/plaintext-task-radar.jpg)

## What it does

`taskradar` reads a folder of Markdown project notes, one note per project. It has two commands.

1. **`taskradar radar`** lists overdue, due-soon, and stale tasks, plus project-level issues such as a passed deadline or an active project with no open task. It prints text, and can also write JSON or a single-file HTML report.
2. **`taskradar lint`** finds task markers and dates that no reader would ever see, and suggests a fix for each. It exits 1 on a finding, so it can guard a notes repository as a pre-commit hook.

The notes stay the source of truth. The tool never edits a note, and every report is regenerated on each run.

## Why it exists

A task tracker built on plain text fails quietly. Type `[/]` instead of `[~]`, or `📅 29 Sep` instead of `📅 2026-09-29`, and the task drops out of every report. Nothing errors. The radar goes quiet exactly when it should not. The linter turns those slips into findings.

## How it is built

- Python 3.11 or newer. The only runtime dependency is PyYAML.
- Six task markers. Only open `[ ]` and in-progress `[~]` tasks are live. A new symbol is an error, never an accidental open task.
- Structural matching. A marker inside backticks, a table cell, or a fenced code block is never a task.
- Reproducible. `--today` fixes the reference date, so the same notes give the same report.
- Small. The core is 438 non-blank, non-comment lines. CI fails the build above 500.

## How it is tested

- **65 tests** with `pytest`, all passing on Python 3.13.5 on 2026-10-10.
- An invented demo set of notes ships with one of each edge case planted in it, and its expected counts written down. The linter reports 7 findings (6 errors, 1 warning). The radar reports 4 overdue, 5 due soon, 1 stale, and 3 project issues, and its output matches a golden file.
- The main checks are paired with planted cases that must fail, including fenced twins and the 7 versus 8 day boundary.

## Limits

- No recurring tasks, times of day, or time zones. Dates are whole calendar days.
- Stale detection needs an `updated` date or git history. File modification times are a weak fallback, because a fresh clone resets them.
- English task signifiers and ISO dates only. Notes are read one at a time, with no cross-note links.
- CI runs on Linux only. macOS was tried by hand, and Windows has not been tried.
- It is a rough prototype (version 0.1.0), not yet used on many real note collections, and not published on PyPI.

MIT licence.
