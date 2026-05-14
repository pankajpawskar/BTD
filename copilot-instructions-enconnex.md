# Copilot Project Instructions — Enconnex (Odoo 18)

You are assisting Pankaj, a beginner Odoo developer.

## Project context
- **Product:** Enconnex ERP
- **Odoo version:** 18.0 Enterprise
- **Python:** 3.11
- **Working directory:** ./custom (this is the ONLY folder Copilot may edit)
- **DO NOT edit:** ./odoo, ./odoo-backup, ./.venv, ./odoo.conf
- **Database:** PostgreSQL via psycopg2
- **Branch model:**
  - Pull from: `Sandbox-18`
  - Work on: `feature/short-task-name`
  - PR target: `Sandbox-18`
  - NEVER touch: `main` (production)

## CRITICAL — Things Copilot must NEVER do

1. **NEVER run shell commands.** Do not suggest or execute:
   - `kill`, `ps aux`, `pkill`
   - `./odoo-bin -u all` or any `-u all` command
   - `dropdb`, `createdb`, `pg_dump`, `pg_restore`
   - `psql -c "CREATE EXTENSION..."` or any raw SQL
   - `brew install` or any system-level install
   - `git push`, `git reset --hard`, `git branch -D`
   - Database resets, migrations, or schema changes
   - Restarting Odoo or any service

2. **NEVER edit these files unless explicitly asked:**
   - `__manifest__.py` (changing depends affects production)
   - `security/ir.model.access.csv`
   - Migration files in `migrations/`
   - Any file outside the specific module the user mentioned

3. **NEVER make multi-file changes.** Stick to the ONE file the user mentions.

4. **NEVER chase tangential errors.** If the user asks to fix bug X and you also see bug Y in the logs, mention bug Y but do NOT fix it. Stay focused.

5. **NEVER use `-u all` or update multiple modules.** Only the specific module needed.

## How to respond

For EVERY code change request:

1. **Explain first, code second.** Plain language summary of what you'll change and why, BEFORE showing code.

2. **Show minimal diff.** Show ONLY the lines being added/changed. Don't paste whole files unless asked.

3. **State the risk.** Every change must end with: "Risk level: LOW / MEDIUM / HIGH" and a one-sentence reason.

4. **Provide test steps.** Plain English steps to test in browser. Example:
   - Open http://localhost:8069
   - Navigate to relevant menu
   - Verify X behavior

5. **Tell user how to apply the change.** Either:
   - "Save the file, restart Odoo with `-u module_name` parameter" (if Python model change)
   - "Save the file, just restart Odoo" (if Python code only)
   - "Save the file, restart Odoo, refresh browser" (if XML view)

6. **Flag senior review needed for:**
   - Schema changes (new fields, removed fields)
   - Security rule changes
   - Cron/scheduled job changes
   - Changes to mail templates
   - API integrations
   - Anything touching `account.*` or `stock.move`

## Code style for Odoo

- Use Odoo ORM (self.env, search, browse, create, write) — NOT raw SQL
- Always inherit existing models with `_inherit` — never replace
- Use `@api.depends` for computed fields, `@api.constrains` for validation
- Fields: snake_case names, include `help=` tooltip, use `tracking=True` for audit-worthy fields
- Views: extend with xpath using `position="after"/"before"/"replace"/"inside"`
- Records: explicit `id` and `model` attributes in `<record>` tags
- Follow PEP 8 for Python

## When unsure

If you're not 100% sure about an Odoo 18 API or behavior, say:
"I'm not certain this is the correct Odoo 18 API. Please verify with official docs at https://www.odoo.com/documentation/18.0 before applying."

Do not invent API calls. Do not guess at field names.

## Output format

Use markdown:
- Headers for sections
- Fenced code blocks with language tag (`python`, `xml`)
- Bold for emphasis
- Inline `code` for filenames/variables

Keep responses focused and short. No filler. No "Certainly!" or "I'll help you with that!" — get straight to the answer.
