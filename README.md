# AI-LLM SQL ASSISSTANT

### Ask your data a question. Get SQL you can review. Turn the answer into insight.

AI-LLM SQL Assistant helps you explore relational data using plain language. Describe what you want to learn, review the SQL generated for your database, then choose whether to run it. Inspect the results as a table or chart, and keep your queries organized for later.

> **Built for clarity:** the assistant generates SQL for your review. Queries are not executed until you approve them.

## What you can do

- **Ask in plain language** and generate schema-aware SQL with Groq, OpenRouter, or Anthropic.
- **Connect to SQLite, MySQL, or PostgreSQL** and browse tables, columns, relationships, and sample rows.
- **Build `SELECT` queries visually** with columns, joins, filters, sorting, row limits, and optional summaries.
- **Review before running.** Inspect SQL, preview available query-plan details, and check detected table and column lineage.
- **Explore results** in a table, an automatically selected chart, or a short numeric summary.
- **Export results to CSV** and revisit, search, favorite, copy, rerun, or delete past queries.
- **Check data quality** with optional null, duplicate, row-count, and numeric-total checks.
- **Create an account** with email verification and manage your account and password.

## How it works

1. **Ask:** Describe the information you need, such as “Show total revenue by product category.”
2. **Ground:** The assistant uses your selected database type and available schema.
3. **Review:** Inspect the generated SQL and its available plan and lineage details.
4. **Approve:** Run the query when you are ready.
5. **Explore:** Review the returned data, chart, and summary; export or save the query if useful.

## Get started

### Requirements

- Node.js compatible with the installed Next.js version
- npm
- A supported database for live data (SQLite is the default)

### Run locally

```bash
git clone <your-repository-url>
cd sql-assistant
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

The app uses SQLite by default and includes a demo schema for exploring the interface. 

## Configuration

### AI provider

Choose a provider with `LLM_PROVIDER` and set its server-side key. Groq is the default.

| Provider | `LLM_PROVIDER` | Required key | Optional model |
|---|---|---|---|
| Groq | `groq` | `GROQ_API_KEY` | `LLM_MODEL` |
| OpenRouter | `openrouter` | `OPENROUTER_API_KEY` | `OPENROUTER_MODEL` |
| Anthropic | `anthropic` | `ANTHROPIC_API_KEY` | — |

If a provider is unavailable or cannot generate a matching query, the app may use its built-in SQL fallback and show a warning. Review all generated SQL carefully before approving it.

### Database

SQLite is the default and stores data in `./data/sql-assistant.db`. Set `SQLITE_DB_PATH` or `DB_PATH` to use a different file.

| Database | Environment variables |
|---|---|
| MySQL | `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` |
| PostgreSQL | `PGHOST`, `PGPORT`, `PGUSER`, `PGPASSWORD`, `PGDATABASE` |

Remote SQL database connections use TLS with certificate verification. Production requires TLS. MongoDB connections also require TLS in production. Do not disable certificate checks for remote databases.

### Accounts and email verification

Server-side accounts, sessions, pending verification codes, and data-quality baselines use MongoDB. Configure:

- `MONGODB_URI` (or `MONGO_URI`)
- Optionally, `MONGODB_DB_NAME` (or `MONGO_DB_NAME`); defaults to `sql_assistant`

Sign-up email verification uses **Mailtrap Email Sandbox**. Set `MAILTRAP_SANDBOX_TOKEN` and `MAILTRAP_SANDBOX_INBOX_ID`; `EMAIL_FROM` is optional. Sandbox messages appear in your Mailtrap inbox and are not delivered to real email addresses.

Without MongoDB, sign-up and login are unavailable. Without Mailtrap Sandbox credentials, email verification cannot be sent.

## Safety and data handling

- SQL runs only after a user approves it.
- SQL mutations are disabled by default. `ALLOW_SQL_MUTATIONS=true` enables them; only do so intentionally and use database-level permissions appropriate to your use case.
- Use least-privilege credentials for MongoDB and your selected SQL database.
- Schema metadata and sample rows are available only to signed-in users. The schema browser samples only the selected table.
- Query history is kept in browser storage and can also be persisted in MongoDB when configured.
- Data-quality checks are explicit and may scan the selected table. They compare selected columns or aggregate values; they do not return row-level data.
- Keep secrets in server-side environment variables. Never put API keys in browser code or commit them to Git.

## Project scripts

| Command | Purpose |
|---|---|
| `npm run dev` | Start the development server |
| `npm run build` | Create a production build |
| `npm start` | Start the production server |
| `npm run lint` | Run ESLint |
| `npm test` | Run the automated tests |
| `npm run migrate:auth-mongodb` | Migrate local SQLite accounts to configured MongoDB |

Authentication tests use a temporary SQLite database and do not modify the app's configured authentication database.

## Notes and limitations

- A demo schema helps explore the interface when a live MySQL or PostgreSQL connection is unavailable; it does not mean queries ran against a live database.
- Creating a PostgreSQL database does not create or populate your application tables. Set up your schema and data before querying.
- SQLite does not support native table and column comments, so descriptions appear only when the selected database provides them.
- Pricing currently redirects to How It Works.
- Accounts in an older local SQLite authentication database are not migrated automatically. Back up the database before using the migration script.
- Query-plan details are estimates, not guarantees of query runtime.

## Tech stack

Next.js · React · TypeScript · Tailwind CSS · SQLite · MySQL · PostgreSQL · MongoDB

## Contributing

Ideas, bug reports, and pull requests are welcome. For changes, please run `npm run lint`, `npm test`, and `npm run build` as appropriate, and avoid including credentials or real user data.

---

**Ask better questions of your data.** Review every query, understand the result, and keep the final say.

