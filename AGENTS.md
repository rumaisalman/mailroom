# AGENTS.md — Mailroom project rules

This file is the single source of rules for any coding agent (Claude Code, Codex, OpenCode, …)
working in this repository. `CLAUDE.md` imports it.

## Purpose

Mailroom is a **learning project**. The learner is working through *Programming Fundamentals*
(Chapter 13) of The AI Agent Factory:
https://agentfactory.panaversity.org/building-ai-workers/programming-fundamentals/

The goal is for the learner to **understand every line**, not just to get working code.
"You read, the agent writes" — the agent writes code, the learner reads, predicts and verifies it.
The learner owns the 10-80-10 split: the learner decides what "correct" means up front and
checks the result at the end; the agent does the middle.

## Language and tooling

- Language: **Python** (latest stable, 3.14, pinned in `.python-version`).
- Project and environment manager: **uv**. Never use `pip`, `venv` or `python` directly.
  - Run code: `uv run python main.py`
  - Add a dependency: `uv add <package>` (ask the learner first; explain why it is needed)
- Type checker: **Pyright** in strict mode → `uv run pyright`
- Linter / formatter: **ruff** → `uv run ruff check .` and `uv run ruff format .`
- Before calling any change "done", run the checks and show the output:
  `uv run pyright` and `uv run ruff check .` must both pass.

## Concepts the learner has covered (up to lesson 13.7)

Only use these in code unless the learner has been taught something new (see next section):

- **13.1** values, strings, lists in `[]`, zero-based indexing, variables with `=`, `print()`
- **13.2** the workbench: runner (`uv run`), tests, type checker (Pyright), linter (ruff);
  type annotations; run vs type check; runtime errors; git basics
  (`status`, `diff`, `log --oneline`, `add`, `commit -m`, `switch -c`, `tag`)
- **13.3** types `str`, `int`, `float`, `bool`, `None`; annotations like `cents: int`;
  reassignment; `int()` conversion and `ValueError`; `+` on text vs numbers and `TypeError`;
  `#` comments; **money is stored as whole cents (`int`), never `float`**
- **13.4** `if` / `else`; comparisons `==`, `>`, `>=`, `<`, `<=`; indentation as blocks;
  `for` loops over a list; filtering with `if` inside a loop; list comprehensions
- **13.5** `and`, `or`, `not`; empty/falsy values (`""`, `None`, `0`); the `or` default trick;
  comparing `YYYY-MM-DD` date strings; `while` loops with a stopping condition; `+=`
- **13.6** functions with `def`, typed parameters and return type (`-> int`); arguments vs
  parameters; `return`; pure/deterministic functions; the signature as a contract; `assert`
- **13.7** lists and `len()`; dictionaries (`dict[str, str]`), keys and values; records as dicts
  read with `record["key"]`; `KeyError`, `.get(key, default)`, `IndexError`;
  `sorted()` / `.sort()`, `key=` with `lambda`, `reverse=True`; text vs number sorting

### Not yet covered (do not use without teaching first)

Reading files (13.8), dates/`datetime` (13.9), tracebacks in depth (13.10), pytest / test files
(13.11), `try` / `except`, classes, modules and packages, imports beyond what `uv init` created,
dataclasses / Pydantic, JSON, `async`, `elif`/`match` beyond what a lesson showed, f-strings with
format specs, `*args`/`**kwargs`, decorators, generators, third-party libraries.

Note: the `if __name__ == "__main__":` line in `main.py` is scaffolding from `uv init`; treat it as
"this runs `main()` when the file is run" until it is taught properly.

## Teaching rule: anything new is taught first with PRIMM-AI+

If a task needs a concept that is not in the covered list above, **stop and teach it before using
it**. Do not quietly slip new syntax into the code. Teach with the book's five steps
(PRIMM-AI+, lesson 13.1) — nothing the agent says is accepted without a check; a run or a test decides:

1. **Predict** — show a small snippet; the learner says what it will do *before* it runs.
   The agent stays quiet and does not hint at the answer.
2. **Run** — run it (with the learner's permission) and show the real output.
3. **Investigate** — compare the prediction with the output; explain lines only when asked.
4. **Modify** — the learner changes one thing, predicts again, runs again.
5. **Make** — the learner specifies their own version; the agent builds it; the learner verifies.

Rule of thumb: **predict before you run, every time.** After teaching, add the concept to the
"covered" list above (with the lesson number if it came from the book).

## How the agent works in this repo

- Keep changes small: one idea per change, so the learner can read all of it.
- Every function has full type annotations; Pyright strict must pass.
- Prefer plain, readable code over clever code. Clear names over short names.
- Money is always `int` cents. Dates are `YYYY-MM-DD` strings until 13.9 is covered.
- Totals and dates come from tested functions, never from a model's reply.
- Use `assert` to check known answers (the known-answer test) until pytest is taught.
- Before running anything, tell the learner what will run and ask them to predict the output.
- Explain errors by quoting the error line and naming the error type (`TypeError`, `KeyError`, …).
- Commit with git in small steps with clear messages; never force-push or rewrite history.
- Never commit secrets (`.env`, API keys, tokens).
