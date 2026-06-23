# dotnet-claude-md-starter

A ready-to-adapt `CLAUDE.md` for a **Clean Architecture .NET** solution.

[`CLAUDE.md`](./CLAUDE.md) is the file [Claude Code](https://docs.claude.com/en/docs/claude-code/overview)
reads at the start of every session — the persistent project context that tells
the agent your stack, where code goes, the rules it must not break, and when it
may act on its own. This repo gives you a solid one to start from instead of a
blank file.

## How to use it

1. Click **Use this template** (or copy [`CLAUDE.md`](./CLAUDE.md) into your repo root).
2. Replace the placeholders — `MyApp`, framework versions, commands — with your
   project's real values.
3. Run `/init` in Claude Code to refine it against your actual code.
4. **Grow it from friction.** Each time the agent gets the same thing wrong,
   don't just correct it in chat — add one line to `CLAUDE.md` so it doesn't
   recur next session. That, not writing it perfectly upfront, is what makes it
   useful over time.

## What's inside

The sample is organized into the sections that earn their keep:

- **Project overview & tech stack** — what the service is, and the exact
  versions/libraries so the agent stops suggesting packages you don't need.
- **Project map** — the solution layout, the inward-dependency rule, and a
  "where to put new code" table.
- **Conventions** — naming and style so generated code reads like the rest.
- **Hard rules** — non-negotiables (typed exceptions, parameterized EF Core,
  tenant filtering, no PII in logs).
- **Definition of done** — a checklist the agent can actually run
  (`dotnet build` / `test` / `format`) before claiming a task is finished.
- **Git rules & autonomy** — how it commits, and where its autonomy stops.

## Background

This template accompanies a series of notes that explain *why* each section
matters and how to adapt it: **[csharp-indonesia.com](https://csharp-indonesia.com)**
(in Bahasa Indonesia). The notes are the canonical explanation; this repo is the
working starting point.

## License

MIT — see [LICENSE](./LICENSE). Copy and adapt freely.
