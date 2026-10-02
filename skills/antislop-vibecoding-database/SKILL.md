---
name: antislop-vibecoding-database
description: "Anti-slop rules for databases: DB type recommendation, explicit relations, documented schema, Indonesian entity names, structured IDs, and indexing. English mirror of docs/database.md."
allowed-tools: Read Write Edit Glob Grep Bash
---

# antislop-vibecoding-database

> Part of the antislop-vibecoding system. Load together with the core skill `antislop-vibecoding`.
> English mirror of the Indonesian source of truth `docs/database.md` (IDs match).
> Note: DB-04 requires **Indonesian** names for tables and columns (the rule is written in English, but the naming convention is Indonesian).

## Scope

DB type selection, schema, relations between tables, entity naming, ID format, and schema documentation.

## Rules

### DB-01 — Recommend & choose the DB type first

- **Tell:** the agent creates a schema or picks a DB type (SQL relational, NoSQL, etc.) without asking the user first.
- **Why:** the DB type drives structure, queries, cost, and maintenance. Guessing means the wrong direction from the start.
- **Fix:** before creating a schema, present DB type options with reasons, pros, and cons (see WF-03), then **ask** which to use. Don't proceed until the user chooses.

### DB-02 — Relations between tables must be explicit

- **Tell:** tables are created standalone without foreign keys; relationships are "kept in the head" or joined manually in app code.
- **Why:** without explicit relations, data integrity is fragile and the structure can't be traced or maintained.
- **Fix:** define primary key → foreign key relations for every relationship. If intentionally non-relational (e.g. NoSQL — see DB-01), write the reason **and** explain how integrity is preserved. "NoSQL allowed" does **not** mean "no relations allowed".

### DB-03 — Schema must be recorded & visible

- **Tell:** the schema exists only in migrations/code; there's no ERD or table documentation.
- **Why:** hard to see and maintain; the next person/agent must dig through migrations just to know the structure.
- **Fix:** record the schema in the repo: an ERD + table docs (name, columns, types, relations). Store it at **one fixed location** — default `db/schema.md` (create the folder if missing) — and record its location in the structure map (WF-09). Keep it updated per WF-07.

### DB-04 — Entity/table names must be in Indonesian

- **Tell:** table/entity names use English or mix languages (e.g. `users`, `order_items`).
- **Why:** project rule: consistency and readability. Mixed languages cause confusion and inconsistency.
- **Fix:** use Indonesian for **table and column** names (e.g. table `pelanggan`, columns `nama`, `tanggal_lahir`), `snake_case`, singular table names. PK columns use `<table>_id` (e.g. `pelanggan_id`); FKs follow the target table's name.

### DB-05 — ID/UID format must be structured

- **Tell:** IDs are large meaningless numbers (e.g. `6000`, `1234567`) or plain auto-increment.
- **Why:** large numbers aren't self-explanatory — hard to read, distinguish, and debug; prone to typos.
- **Fix:** use a consistent UID format `<prefix><YYYYMMDD><4 random digits>` (no separator):
  - **prefix** = entity code, lowercase English (e.g. `cust`, `ord`, `prd`)
  - **YYYYMMDD** = record creation date
  - **4 random digits**
  - example customer: `cust202601014821`
  - **IDs must be unique.** Since 4 random digits can collide, regenerate on conflict or check the DB before saving (retry).
  - **Record the prefix per entity** (see DB-03) for consistency.

  | Entity | Prefix | Example ID |
  |---|---|---|
  | pelanggan | `cust` | `cust202601014821` |
  | pesanan | `ord` | `ord202601014821` |
  | produk | `prd` | `prd202601014821` |

### DB-06 — Index for large-data queries

- **Tell:** tables with many rows are queried/filtered/sorted without indexes, so pages are slow to render large data.
- **Why:** without an index, the DB full-scans every row per query. More data, slower.
- **Fix:** add **indexes** on columns often used for filtering, joining, and sorting (foreign keys, `created_at`, search columns, etc.). Record indexes in the schema (DB-03). For large data, also limit rows per view (pagination). Don't index every column without reason — indexes add write cost.

## Checklist

All answers must be **yes**:

- [ ] DB type recommended with reasons, pros, cons, and the user chose. (DB-01)
- [ ] All table relations are explicit (PK → FK), or the non-relational reason is written. (DB-02)
- [ ] Schema recorded: an ERD + table docs exist in the repo. (DB-03)
- [ ] Table & column names are Indonesian, `snake_case`, singular. (DB-04)
- [ ] All IDs use `<prefix><YYYYMMDD><4 random digits>`, are unique, and prefixes are recorded. (DB-05)
- [ ] Frequently queried/filtered/sorted columns are indexed, and large data is paginated. (DB-06)
