# Local data directory

This directory is for sensitive, local-only runtime material. Git ignores everything here except this guide.

Create `data/inputs/` and place personal bank statements there for future ingestion. Do not commit PDFs, exported transactions, OCR text, SQLite databases, temporary files, or logs.

The application will use this directory by default when `EXPENSE_DATA_DIR=data`. You may point that variable to another local directory if preferred.
