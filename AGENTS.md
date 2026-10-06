# Contributor Guide

## Project direction

Personal Expense Tracker is a privacy-first personal finance application. It will use Go for the application, Go's `html/template` package for server-rendered pages, SQLite for local persistence, and a provider-agnostic boundary for optional cloud AI extraction and categorisation.

Keep application features local by default. The database, imported statements, derived text, and generated reports belong on the user's device. Cloud AI is optional and must be explicitly configured by the user.

## Financial-data safety

- Never commit bank statements, PDFs, screenshots, CSV exports, OCR output, transaction exports, SQLite databases, logs containing financial data, API keys, or credentials.
- Use `data/` only for local runtime material. The only tracked file below it is `data/README.md`.
- Do not add real financial records to tests, examples, documentation, issue reports, or fixtures. Use clearly synthetic or redacted data instead.
- Keep credentials in local environment variables or an ignored `.env` file. Commit only non-secret placeholders in `.env.example`.
- Do not add telemetry, analytics, or background uploads. Any future cloud request must require explicit user configuration and explain what statement content is sent to the configured provider.
- Treat temporary extraction files with the same sensitivity as the original statement; store them under ignored local storage and remove them when no longer needed.

## Implementation conventions

- Keep Go code formatted with `gofmt` and prefer the standard library unless a dependency provides a clear benefit.
- Render the web UI with `html/template`; avoid embedding secrets or statement data in client-side assets.
- Keep SQLite access behind a small storage layer so schema and persistence details do not leak throughout the application.
- Keep AI-provider code behind an interface. Provider credentials, endpoints, and request formatting must not be coupled to statement ingestion or storage code.
- Prefer explicit configuration and actionable errors over implicit provider selection or silent fallbacks.

## Validation before contributing

- Run relevant Go tests and formatting checks once application code exists.
- Review `git status` and staged diffs for sensitive files before every commit.
- Verify `.gitignore` still excludes runtime data, PDFs, databases, logs, and local credentials when changing project tooling.
