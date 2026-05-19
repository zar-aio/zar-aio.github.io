---
layout: post
title: "Things I Wish I Knew Before Writing My First CLI Tool"
date: 2026-05-10
tags: [cli, python, tooling]
description: "Practical lessons learned the hard way while building command-line tools people actually want to use."
---

Command-line tools are incredibly satisfying to build. They're fast, composable, and — when done right — feel almost magical. But there are a handful of things I wish someone had told me before I started.

## 1. Design the help text first

Before writing a single line of logic, write the `--help` output you want to see. This forces you to think about the interface from a user's perspective and doubles as a lightweight spec.

```
Usage: mytool [OPTIONS] INPUT

  Process INPUT and do something useful.

Options:
  -o, --output PATH  Where to write results. [default: stdout]
  -v, --verbose      Enable verbose logging.
  --help             Show this message and exit.
```

## 2. Use an argument parsing library

Don't parse `sys.argv` manually. In Python, reach for [Click](https://click.palletsprojects.com) or [Typer](https://typer.tiangolo.com). They handle edge cases you haven't thought about yet, generate help text automatically, and make testing trivial.

## 3. Respect `stdout` and `stderr`

Results go to `stdout`. Status messages, warnings, and errors go to `stderr`. This lets users pipe output without capturing noise.

```python
import sys

print("Processing...", file=sys.stderr)
print(result)           # goes to stdout
```

## 4. Exit codes matter

`0` means success. Anything else means failure. If your tool is going to be used in scripts (and it will), wrong exit codes silently break pipelines.

## 5. Test with real users early

Your intuition about what is "obvious" is wrong. Give the tool to someone else after they've only read the help text. Watch where they get stuck. That's your next thing to fix.

---

Building a great CLI is an exercise in empathy. The user just wants to get something done. Your job is to stay out of the way.
