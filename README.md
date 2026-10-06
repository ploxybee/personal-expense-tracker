# Personal Expense Tracker

A privacy-first, local personal-finance tracker for consolidating spending across banks. It is intended to ingest personal statements, use optional AI assistance to extract and categorise transactions, retain the resulting data in a local SQLite database, and present spending insights through a Go server-rendered interface.

> **Project status:** This repository is currently initialized with its privacy and configuration conventions. It does not yet contain an application server, importer, database schema, or AI integration.

## Intended workflow

1. Put personal statement PDFs in the local `data/inputs/` directory.
2. The future importer extracts transactions and sends statement content to a user-configured AI provider only when AI assistance is enabled.
3. The application normalises and categorises transactions, then stores them in a local SQLite database.
4. Go-rendered pages show spending history, categories, and cross-bank insights.

## Privacy and cloud AI

The application is designed to keep its database, imported statements, extracted text, and generated reports on your device. The repository ignores the local `data/` directory and all PDFs to help prevent accidental commits of financial information.

AI processing is configurable rather than built in. If you configure a cloud AI provider, statement content and/or extracted text needed for that request will leave your device and be sent to that provider. Review the provider's privacy terms before enabling it. The application must require explicit configuration before making any cloud request, and it will not include telemetry, analytics, or background uploads.

Never commit statement PDFs, transaction exports, SQLite files, API keys, or `.env` files.

## Getting started

The planned implementation targets Go 1.23 or later and uses Go's `html/template` package for the frontend.

```sh
cp .env.example .env
mkdir -p data/inputs
```

Edit the local `.env` file only if you intend to enable cloud AI:

| Variable              | Purpose                                                                               |
| --------------------- | ------------------------------------------------------------------------------------- |
| `EXPENSE_AI_PROVIDER` | Name of the cloud AI provider to use. Leave empty to disable AI calls.                |
| `EXPENSE_AI_API_KEY`  | Credential for the configured provider. Keep this secret.                             |
| `EXPENSE_AI_MODEL`    | Provider-specific model identifier.                                                   |
| `EXPENSE_DATA_DIR`    | Local directory for statements, SQLite data, and temporary files. Defaults to `data`. |

`.env` and all contents of `data/` other than its guide are ignored by Git. PDFs are ignored everywhere in this repository.

## Architecture direction

- **Application:** Go.
- **Frontend:** server-rendered HTML using `html/template`.
- **Storage:** local SQLite, isolated behind a storage layer.
- **AI:** a provider-agnostic interface, configured explicitly through the environment.
- **Safety:** no real financial fixtures, no embedded credentials, and no automatic data transmission.

## Roadmap

- Define the transaction, account, and category data model.
- Add local SQLite migrations and a storage layer.
- Implement a statement-ingestion pipeline with secure temporary-file handling.
- Add configurable AI extraction and categorisation providers.
- Build template-rendered dashboards for spending and category insights.
