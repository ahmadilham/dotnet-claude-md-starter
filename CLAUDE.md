<!--
  Sample CLAUDE.md for a Clean Architecture .NET solution.
  Replace the placeholders (MyApp, versions, commands) with your project's
  real values, then run `/init` in Claude Code to refine it against your code.
  Grow this file from friction — see the README for how.
-->

# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project overview

**MyApp** is a <one-line description of what this service does>. It follows
Clean Architecture: dependencies point inward, and the domain has no knowledge
of frameworks, the database, or the web.

## Tech stack

| Layer | Technology |
|-------|------------|
| Runtime | .NET 9 |
| Web | ASP.NET Core Minimal API |
| Data | EF Core 9 + PostgreSQL (Npgsql) |
| Tests | xUnit + FluentAssertions |
| Validation | FluentValidation |

Target framework is `net9.0`. Do not introduce a dependency without a reason —
prefer what is already referenced.

## Project map

```
src/
  MyApp.Domain         — entities, value objects, domain rules. Depends on NOTHING.
  MyApp.Application    — use cases, interfaces, DTOs. Depends on Domain only.
  MyApp.Infrastructure — EF Core, external services. Depends on Application + Domain.
  MyApp.Api            — composition root, endpoints. Depends on all of the above.
tests/
  MyApp.UnitTests
  MyApp.IntegrationTests
```

**Dependency rule:** dependencies point inward. Domain must never reference
Application, Infrastructure, or Api. To call out (e.g. the database) from a use
case, declare an interface in Application and implement it in Infrastructure.

### Where to put new code

| If building...            | Put it in...                        |
|---------------------------|-------------------------------------|
| Entity / domain rule      | `MyApp.Domain`                      |
| Use case / handler        | `MyApp.Application/Features/<Name>` |
| Repository implementation | `MyApp.Infrastructure/Persistence`  |
| External API client       | `MyApp.Infrastructure/Integrations` |
| HTTP endpoint             | `MyApp.Api/Endpoints`               |
| Shared DTO / contract     | `MyApp.Application/Contracts`       |

## Commands

```bash
dotnet build                       # build the solution
dotnet test                        # run all tests
dotnet format --verify-no-changes  # check formatting
dotnet run --project src/MyApp.Api # run the API locally
dotnet ef migrations add <Name> --project src/MyApp.Infrastructure --startup-project src/MyApp.Api
dotnet ef database update --project src/MyApp.Infrastructure --startup-project src/MyApp.Api
```

## Conventions

- File-scoped namespaces. One public type per file.
- Types: PascalCase. Locals/params: camelCase. Constants: PascalCase.
- Prefer `record` for DTOs and value objects, `sealed class` for services.
- Async methods end with `Async` and take a `CancellationToken` last.
- Return `Result<T>` for expected failures; throw only for truly exceptional cases.

## Hard rules

- Never throw bare `new Exception(...)` in domain code. Use a typed/domain
  exception or return `Result<T>`, so callers can classify the failure.
- All EF Core queries are parameterized. Never build SQL with string
  interpolation. Use `FromSqlInterpolated` (not `FromSqlRaw` + concatenation).
- In a multi-tenant table, every read/update/delete filters by `TenantId` — not
  by primary key alone. Prefer an EF Core global query filter.
- Never put raw user input or PII in exception messages or logs. Reference
  metadata only: field name, row index, error code — never the value itself.

## Definition of done

Run before reporting a task as complete:

- [ ] `dotnet build` passes with zero warnings (warnings-as-errors is on)
- [ ] `dotnet test` passes
- [ ] `dotnet format --verify-no-changes` is clean
- [ ] New public behavior has a test
- [ ] No secrets, connection strings, or PII in code, logs, or commits

Never report "done" without running the build and tests.
Workflow: implement → build → test → fix → re-run → only then report.

## Git rules

- Never work on `main` directly. Always branch first.
- Branch naming: `<type>/<short-description>`, e.g. `feat/user-export`.
- Conventional commits: `feat:`, `fix:`, `refactor:`, `docs:`, `test:`, `chore:`.
- Never force-push. Never rebase a shared branch.
- NEVER commit or push without explicit confirmation. Stage the changes, show a
  summary, and wait for me to say "commit" or "push".

## Autonomy

Act without asking for standard work: writing code, refactoring, adding tests,
fixing bugs within the stated scope. Don't ask "shall I continue?" — keep going
until the task is done or you are genuinely blocked.

Ask first only for:

- Architectural changes (new project, swapping a library, changing a schema)
- Business/product decisions (what a feature should do)
- Security-sensitive changes (auth, secrets, permissions)
- Anything destructive or hard to reverse
