---
title: "ollama-truncation-guard: refuse answers built on a truncated prompt"
excerpt: "A zero-dependency proxy in front of a local Ollama model server. It checks every answer against the prompt the model actually saw, and refuses it if the prompt was silently cut."
collection: portfolio
group: "Tools and software"
order: 1
tags: [Python, LLM, Ollama, testing]
header:
  teaser: projects/ollama-truncation-guard.jpg
teaser_alt: "Terminal output showing the proxy refusing a truncated answer with HTTP 502"
---

<a class="project-cta" href="https://github.com/Adrian-I-lab/ollama-truncation-guard">View the code on GitHub</a>

![Terminal output showing the proxy refusing a truncated answer with HTTP 502](/images/projects/ollama-truncation-guard.jpg)

## What it does

`ollama-truncation-guard` sits between a client and a local [Ollama](https://ollama.com) model server. Every request passes through it.

1. **Before the model runs**, it estimates how many tokens the prompt holds. If the prompt cannot fit in the context window, it refuses at once with HTTP 413, so no GPU time is wasted.
2. **After the model answers**, it reads `prompt_eval_count`, the number of prompt tokens the model actually evaluated. If that is materially below what was sent, it refuses the answer with HTTP 502 and logs both numbers.
3. **Otherwise** the answer passes through unchanged.

## Why it exists

A large open-weights model served with a 131,072-token window was sent prompts of about 137,000 and 168,000 tokens. Both times the server reported 65,538 evaluated tokens, about half the window. There was no error and no warning. The model answered fluently from the part of the prompt it kept.

`prompt_eval_count` is the only signal the API gives, so the guard checks it at the one place every request passes through.

## How it is built

- Python 3.10 or newer, standard library only. Zero dependencies, so there is nothing to install, vet, or pin.
- `guard.py` holds the pure checks (52 lines). `proxy.py` holds the HTTP proxy (145 lines). A CI gate keeps the two under 200 lines.
- Four checks: the ratio of evaluated to expected tokens, a "half-window" signature, a count larger than the window, and a missing count.
- Streaming is supported. The default mode buffers the stream and replays it only if the check passes.

## How it is tested

- **43 tests** with Python's `unittest`, run in CI on Python 3.10 and 3.12.
- The tests run against a fake Ollama server that truncates on purpose. Each key test sends the same request to a truncating fake (must be refused) and an honest fake (must pass).
- A **control** runs the truncating fake with the guard switched off, and the truncated answer gets through. That proves the guard, not the fake, does the refusing.
- Checked against a real server (Ollama 0.40.2, `qwen2.5:0.5b`). A prompt of about 2,500 tokens at a 512-token window came back with 258 evaluated tokens, and the proxy refused it.

## Limits

- Without a client-supplied count, the expected size is a deliberately low estimate (characters divided by 6), not a tokenizer. Code and non-English text tokenise differently.
- Only `/api/generate` and `/api/chat` are checked. The OpenAI-compatible `/v1/*` endpoints are forwarded unchecked.
- It is a localhost tool for one user. There is no authentication and no TLS.

MIT licence.
