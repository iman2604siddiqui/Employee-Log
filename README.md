# Keycard Employee Onboarding Assistant

A full-stack extension of the existing Keycard frontend. The original `index.html` remains the main employee workspace.

## PostgreSQL setup

The backend uses the `pg` package and reads one connection string from `DATABASE_URL`. The expected format is:

```text
postgresql://USER:PASSWORD@HOST:5432/DATABASE
```

The example value targets a local database named `keycard` and does not create that database automatically. PostgreSQL must be installed, running, and the database must exist before initialization.

1. Copy `.env.example` to `.env`.
2. Set `DATABASE_URL` to your PostgreSQL connection string and replace `JWT_SECRET` with a long random value.
3. Create the database if needed:

	```bash
	createdb keycard
	```

4. Initialize all tables and indexes:

	```bash
	psql "$DATABASE_URL" -f schema.sql
	```

5. Verify the database connection through the running API:

	```bash
	curl http://localhost:3000/api/health
	```

The health endpoint returns HTTP 200 with `database: "connected"`, or HTTP 503 with `database: "unavailable"`.

## Development seed

The seed creates synthetic users and document metadata only. It never creates production credentials and does not create fake downloadable files. Set temporary development passwords in your shell or `.env` first:

```bash
export SEED_EMPLOYEE_PASSWORD='use-a-local-password'
export SEED_HR_PASSWORD='use-a-local-password'
export SEED_IT_PASSWORD='use-a-local-password'
npm run db:seed
```

Seed users:

- `demo.employee@example.test` as `employee`
- `demo.hr@example.test` as `hr_admin`
- `demo.it@example.test` as `it_support`

Upload the actual approved files through the authorized document endpoint after seeding to create persistent downloadable versions.

## Run locally

1. Run `npm install`.
2. Complete PostgreSQL setup above.
3. Run `npm start`.
4. Open `http://localhost:3000`.

The server supports employee, HR admin, and IT support roles. If `OPENAI_API_KEY` is absent, the assistant uses a deterministic PostgreSQL full-text retrieval fallback that only answers from indexed document chunks.
