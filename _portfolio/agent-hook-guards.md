---
title: "agent-hook-guards: keep an AI coding agent out of protected paths"
excerpt: "Small, tested guards for AI coding agents. A pre-tool hook blocks file tools and shell commands that reach a protected area, and a git hook blocks secrets and pushes to unlisted remotes."
collection: portfolio
group: "Tools and software"
order: 2
tags: [Python, Bash, AI agents, git, testing]
header:
  teaser: projects/agent-hook-guards.jpg
teaser_alt: "Terminal output showing the guard blocking a read through a symlink and a recursive grep, allowing an echo, and the test suite passing"
---

<a class="project-cta" href="https://github.com/Adrian-I-lab/agent-hook-guards">View the code on GitHub</a>

![Terminal output showing the guard blocking a read through a symlink and a recursive grep, allowing an echo, and the test suite passing](/images/projects/agent-hook-guards.jpg)

## What it does

`agent-hook-guards` is a set of small guards for an AI coding agent. Version 0.1 has two.

1. **A protected-paths hook.** It runs as a Claude Code `PreToolUse` hook, so it sees every tool call before it runs. It blocks file tools and shell commands whose path operands reach a protected area. That covers symlinks, `..`, relative paths, redirects, globs, `$(...)` and `sh -c` strings, symlink-following flags such as `grep -R`, and recursive walks over a parent folder.
2. **A git guard.** Pre-commit, commit-msg, and pre-push hooks block secret-shaped content and file names, optional private terms, and pushes to remotes that are not listed.

Mention is not access. The hook blocks a path in operand position, not a string in text. Notes, `echo` text, and commit messages that name the protected area pass.

## Why it exists

Agent permission rules are usually scoped to tools. A rule that denies the Read tool on a folder does not stop `cat`, a redirect, a glob, a symlink, or `grep -R` reaching the same files through the shell. One hook that sees every call can apply one rule to all of them.

Git is the other common leak. An agent told to "commit your work" commits whatever is staged. A git hook is the last check before content enters history.

## How it is built

- Python 3.9 or newer, standard library only. No third-party packages.
- The git guard is a bash script. It runs on bash 3.2 or later with git and the standard text tools.
- A missing, unreadable, or invalid config blocks every call. With no remote configured, every push is refused.
- The suggested hook command wraps the guard in `sh -c`, so a missing guard file or a Python that will not start still blocks instead of silently allowing.
- Anything the small shell tokeniser cannot read is blocked.

## How it is tested

Run with `bash run_tests.sh`. On 2026-10-10 every step passed on Python 3.9.6 and 3.13.5.

- **49 unit tests** with Python's `unittest`, run on both interpreters.
- **54 protected-path cases**, 32 that must block and 22 that must be allowed, with 0 mismatches.
- **38 git guard cases** (22 block, 16 allow), with 0 failures. The allowed cases include innocent hyphenated words that look like secrets to a careless pattern.
- A **mutation check** reruns the 92 cases against an always-allow and an always-block stand-in. Both stand-ins fail cases, so the tests can go red.
- CI runs the same command on Ubuntu and macOS with Python 3.9, 3.11, and 3.13.

## Limits

- It is coordination for a cooperative but fallible agent. It is **not a sandbox** and **not a security boundary**.
- It does not stop a script written to disk and then run, a path built at run time inside an interpreter, or an obfuscated command.
- It does not stop `git commit --no-verify`, a secret with no recognisable prefix, anything already in history, or a fresh clone where the git hooks were never installed.
- A live run inside Claude Code on 2026-10-10 covered reads only. Writes, deletes, and moves into the protected area are covered by the test cases, not by a live run.
- It is a v0.1 prototype. Expect rough edges and breaking changes.

## In progress

The [`p3-next`](https://github.com/Adrian-I-lab/agent-hook-guards/tree/p3-next) branch adds a read-only guard for remote shells, a labelled synthetic prompt-injection corpus with a simple tripwire, and a PCA and UMAP map of the corpus embeddings. These are prototypes and not yet part of a release.

MIT licence.
