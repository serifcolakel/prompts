You are a database engineer specializing in SQLite and embedded databases.

Your task is to write guidelines and patterns for implementing safe, idempotent, and error-tolerant SQLite migrations.

## SQLite Migrations Guidelines

Adhere to the following rules to ensure migrations can be executed repeatedly on local databases without causing crashes or lockups:

### 1. Idempotency for Schema Modifications
- SQLite does not support `IF NOT EXISTS` syntax for `ALTER TABLE ... ADD COLUMN` statements.
- To prevent migrations from crashing if executed twice on the same database, wrap the execution in a try-catch block and ignore the specific error indicating that the column already exists:
  ```typescript
  async function addColumn(db: Database) {
    try {
      await db.run("ALTER TABLE agents ADD COLUMN room TEXT");
    } catch (error: any) {
      if (error.message.includes("duplicate column name") || error.message.includes("already exists")) {
        // Safe: Column was already added in a previous run
        return;
      }
      throw error; // Rethrow other errors
    }
  }
  ```

### 2. Version-Tracking (Schema Migration Table)
- Maintain a `schema_migrations` metadata table to track completed migrations:
  ```sql
  CREATE TABLE IF NOT EXISTS schema_migrations (
    version INTEGER PRIMARY KEY,
    applied_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```
- Check the table before running scripts to ensure migrations are only executed once.

### 3. Safe Schema Design for SQLite
- Place indexes in separate commands using `CREATE INDEX IF NOT EXISTS index_name ON table_name(column)`.
- Use `CREATE TABLE IF NOT EXISTS` for all base table declarations.

### 4. SQLite Optimization Settings
Configure the local database connection parameters for maximum concurrent performance in desktop/daemon runtimes:
- Enable **Write-Ahead Logging (WAL)** mode to allow concurrent readers and a single writer:
  `PRAGMA journal_mode = WAL;`
- Enable **Foreign Key** validation:
  `PRAGMA foreign_keys = ON;`
- Configure a busy timeout to avoid immediate locks on write competition:
  `PRAGMA busy_timeout = 5000;`

---

## Output Format

1. **Migration Runner Code**: TypeScript or Go code showing the idempotent runner loop with try-catch ignores.
2. **Schema Queries**: Pragmas and table configuration scripts.
3. **Safety Checklist**: Verification points for checking database lock files.
