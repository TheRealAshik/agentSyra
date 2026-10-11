# Database Safety

## Access and discovery

- Use a database only through an available, authenticated, authorized connection. GitHub authorization does not imply database authorization.
- Identify the database engine, project, environment (development/staging/production), and permissions before interacting.
- Prefer read-only inspection first: schema, migrations, relevant constraints, indexes, relationships, policies, and bounded queries.
- Use explicit columns and limited results. Avoid dumping tables or unrelated personal data.
- If no live database connection exists, inspect repository models and migrations, but label conclusions as repository-derived rather than live database facts.

## Schema and data changes

- Prefer version-controlled migrations and reproducible scripts over undocumented manual edits.
- Check application compatibility, primary/foreign keys, constraints, indexes, access policies, and rollback/forward-fix strategy relevant to the change.
- Obtain explicit, operation-specific approval before destructive or hard-to-reverse operations, production schema/data changes, broad updates/deletes, resets, permission/security-policy changes, or irreversible migrations.
- Approval of a code plan does not automatically authorize a separate database write.
- Never weaken authentication, authorization, row-level security, or audit controls merely to make a feature work.
- Do not use blanket deletes or updates without a verified scope and an approved rollback/recovery approach.

## Secrets and privacy

- Never expose passwords, tokens, keys, private connection strings, or sensitive records in chat, logs, generated files, commits, or PRs.
- If a potential secret is discovered, report its presence without repeating its value. Follow the repository's secret-handling process.
- Minimize personal data access and avoid retrieving data unrelated to the approved task.

## Provider-specific guidance

Use provider-specific syntax and practices only when the provider and connection are confirmed. For example, apply PostgreSQL/RLS guidance to an authorized Supabase connection, or SQLite-compatible SQL to Cloudflare D1, only when those are actually the target tools. Do not assume that every integration supports both reading and writing.

## Report

State which project/environment was accessed, whether operations were read-only or mutating, the schema/data scope, migration/check results, and any rollback caveat. Do not expose secrets or personal records in the report.
