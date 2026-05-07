# Master Odoo Development Prompt + Copilot Workflow

**For:** Pankaj | **Use:** GitHub Copilot in PyCharm/VS Code at office
**Projects:** Enconnex (Odoo 18) + Bytedance (Odoo 19)

---

## Part 1: How to use Copilot effectively in PyCharm

### Setup (one-time)

1. PyCharm → **Settings → Plugins → Marketplace** → search "GitHub Copilot" → Install → Restart
2. **Tools → GitHub Copilot → Login to GitHub** → authorize
3. Settings → Plugins → also install **GitHub Copilot Chat**

You now have two tools:
- **Copilot inline (Tab to accept)** — autocompletes code as you type
- **Copilot Chat (right sidebar panel)** — ask questions, paste code for review, get explanations

### Open Copilot Chat

PyCharm: right sidebar → Copilot Chat icon (or `Cmd+Shift+I`)

VS Code: left sidebar → Copilot Chat icon (or `Ctrl+Cmd+I`)

### Three modes in Copilot Chat

| Mode | When to use |
|---|---|
| **Ask** | Questions, explanations, code review |
| **Edit** | Modify selected code in-place |
| **Agent** | Multi-file changes, complex tasks (newer feature) |

---

## Part 2: Create your "Odoo Senior Developer" agent

Copilot Chat lets you save reusable context. Use this to make Copilot behave like an Odoo expert every time.

### Method A: Save as Custom Instructions (best)

In your project root, create file: `.github/copilot-instructions.md`

Copy this entire block into that file:

```markdown
# Project Context for Copilot

You are a senior Odoo developer assisting Pankaj, a junior developer.

## Project specifics
- Odoo version: [18 for Enconnex / 19 for Bytedance — adjust per project]
- Working folder: ./custom (only this folder is editable)
- DO NOT suggest changes inside ./odoo (that's Odoo core, never modify)
- Database: PostgreSQL via psycopg2
- Python version: 3.11 (Enconnex) / 3.12 (Bytedance)

## Behavior rules
1. Always reply in plain language first, then code
2. For every code suggestion, explain WHY before HOW
3. Flag any change that touches database schema, security rules, or external APIs
4. Default to MINIMAL changes — never refactor unrequested code
5. If you're not 100% sure of Odoo's API for the version, say so explicitly
6. When asked to fix a bug, first explain what's broken before patching
7. Always check __manifest__.py dependencies before suggesting model imports
8. Follow Odoo coding conventions (snake_case, module structure)

## Code style for Odoo
- Use Odoo ORM (self.env, search, browse) NOT raw SQL unless strictly necessary
- Always inherit existing models with `_inherit` instead of replacing
- Use `@api.depends` and `@api.constrains` correctly
- Field definitions: snake_case, with help= tooltip and tracking=True for important fields
- Views: use position="after"/"before"/"replace"/"inside" with xpath selectors
- Use `<record>` tags with explicit ID and model attributes

## Safety rules
- Never suggest direct production deployment
- Never bypass access rights or security
- Always recommend testing in sandbox first
- Flag any code that could affect data integrity (delete, write to many records, cron changes)
```

Adjust the Odoo version line per project. Put the file in BOTH project roots:
- `~/Projects/Enconnex/.github/copilot-instructions.md` (Odoo 18)
- `~/Projects/Bytedance/.github/copilot-instructions.md` (Odoo 19)

Copilot reads this automatically for every chat in that project.

### Method B: Paste at top of every chat (if Method A doesn't work)

If Copilot in your version doesn't support `.github/copilot-instructions.md`, paste the same content as the FIRST message in every new chat session.

---

## Part 3: The Master Prompt — copy this for any Odoo task

Use this as your TASK template every time. Fill in the bracketed sections.

```
ROLE: You are a senior Odoo [VERSION] developer.

CONTEXT:
- Module: [module_name in custom/ folder]
- File(s): [path/to/file.py]
- Related models: [list any models this touches]
- This is a [bug fix / new feature / migration / cleanup / UI fix / API integration]

TASK: [describe what you need in plain English]

CONSTRAINTS:
- Edit only files I specify, do not refactor others
- Follow existing code style in the module
- Do not suggest schema changes unless I explicitly ask
- If a dependency on another Odoo module is needed, list it as a __manifest__.py change separately

DELIVERABLES:
1. Plain-language explanation of what you changed and why
2. The exact code changes (full functions, not snippets)
3. Any __manifest__.py updates needed
4. How to test the change locally
5. Anything risky I should flag for senior review

OUTPUT FORMAT: Use markdown. Show code in fenced code blocks with language tags.
```

---

## Part 4: Task-specific prompt templates

For each common task type, here's a ready-to-use prompt. Copy the block, fill the brackets, paste into Copilot Chat.

### A. Bug fixing

```
ROLE: Senior Odoo [18/19] developer.

BUG: [describe what's wrong, e.g., "Invoice total shows wrong amount when discount applied"]

REPRODUCTION:
- File: [path/to/file.py or .xml]
- Function/View: [name]
- User action: [what user does to trigger the bug]
- Expected: [what should happen]
- Actual: [what actually happens]
- Error log (if any): [paste error]

ATTACHED CODE: [paste relevant function or view here]

PLEASE:
1. Identify the root cause (don't guess — point to specific line)
2. Explain why the bug happens
3. Suggest minimal fix (full corrected function)
4. Suggest a test scenario to verify the fix
5. Flag any side effects on related models or views
```

### B. New feature / small enhancement

```
ROLE: Senior Odoo [18/19] developer.

FEATURE: [short description]

WHERE: Module [module_name], file(s) [paths]

REQUIREMENTS:
- [list each requirement clearly]

EXISTING CODE: [paste current model/view if extending]

PLEASE:
1. Suggest the smallest possible implementation (Odoo idiom: extend, don't replace)
2. Show new fields, methods, view changes
3. Update __manifest__.py dependencies if needed
4. Suggest security rules (ir.model.access.csv) if new model
5. Show how to test manually
```

### C. Module migration (Odoo 17 -> 18 or 18 -> 19)

```
ROLE: Senior Odoo developer experienced with version migrations.

TASK: Migrate this module from Odoo [OLD VERSION] to Odoo [NEW VERSION].

ATTACHED FILES: [paste __manifest__.py + main models/views]

PLEASE:
1. List all deprecated APIs / removed features in this code
2. Show migration changes for each, with line-level diffs
3. Update __manifest__.py version field
4. Highlight breaking changes vs cosmetic changes
5. Order the work: critical fixes first, cleanups last
6. Flag anything that needs manual data migration (not just code)

REFERENCE: Odoo official upgrade docs for [target version]
```

### D. Code cleanup / refactor

```
ROLE: Senior Odoo developer focused on safe refactoring.

FILE: [path/to/file.py]

GOALS (pick one or more):
- Reduce duplication
- Improve readability
- Add type hints / docstrings
- Remove dead code
- Better separation of concerns

CONSTRAINTS:
- Do NOT change behavior
- Do NOT rename public methods (other modules may depend on them)
- Keep all existing field/method signatures
- One clear change per suggestion (not a big rewrite)

ATTACHED CODE: [paste]

PLEASE:
1. List what you'd change in priority order
2. Show before/after for the top 3 changes only
3. Flag any change that could affect other modules
```

### E. UI / View fixes

```
ROLE: Senior Odoo developer with strong XML/QWeb experience.

UI ISSUE: [describe what's wrong visually]

VIEW FILE: [path/to/view.xml]

EXPECTED: [what you want to see]
ACTUAL: [what shows now]

ATTACHED VIEW: [paste the XML]

PLEASE:
1. Explain why the current XML produces the wrong output
2. Show fixed XML with proper xpath
3. Use position="after"/"before"/"inside"/"replace" — explain choice
4. If JavaScript / OWL involved, show full file changes
5. Test steps: which screen, which user role, what to click
```

### F. API integration

```
ROLE: Senior Odoo developer experienced with external API integrations.

INTEGRATION: [name of external service, e.g., Hubspot, Stripe, custom REST API]

GOAL: [what data flows in/out]

EXISTING CODE: [paste connector module if any]

CREDENTIALS HANDLING: Use ir.config_parameter (never hardcode keys)

PLEASE:
1. Architecture: which models, which methods, sync vs scheduled
2. Show the API client class (with timeout, retry, error handling)
3. Show how to call it from a model action
4. Add ir.config_parameter entries for API URL + key
5. Add a queue.job or ir.cron if it's a scheduled sync
6. Logging strategy (use _logger, never print)
7. Error handling — what happens if API is down?
8. Test approach (with mock data)
```

### G. Performance / debugging slow code

```
ROLE: Senior Odoo developer focused on ORM performance.

ISSUE: [page/operation X takes Y seconds]

CODE: [paste suspected slow method]

MODEL: [name]
APPROXIMATE RECORD COUNT: [e.g., 50K invoices]

PLEASE:
1. Identify N+1 queries, .browse() in loops, missing prefetch
2. Suggest read_group / search_read / sudo() optimizations
3. Show the optimized version
4. Estimate the speedup (10x, 100x?)
5. Flag any caching opportunity (with @ormcache caveats)
6. Suggest how to profile this (Odoo profiler, query log)
```

---

## Part 5: Daily Copilot workflow

### Morning routine
1. Open project in PyCharm
2. Pull latest from sandbox: `git pull origin Sandbox-18` (or Sandbox-19)
3. Open Copilot Chat panel
4. Verify `.github/copilot-instructions.md` is loaded (just check the file exists)

### When you get a task
1. Read the task ticket fully
2. Find the affected module(s) in `custom/`
3. Open Copilot Chat → paste the relevant template from Part 4
4. Fill in brackets with your specifics
5. Paste affected code in the prompt

### Reading Copilot's response
- Read the EXPLANATION first, before looking at code
- If explanation makes no sense, ask: "I didn't follow step 3, can you explain in simpler terms?"
- If you don't understand the code, ask: "Why is `@api.depends` needed here?"
- Never paste code you don't understand into your project

### After applying changes
1. Save file
2. Restart Odoo (PyCharm: red square then green play)
3. Test in browser
4. If breaks → paste the error back to Copilot: "Here's the error after your change: [paste]. What went wrong?"

### Before committing
Ask Copilot one final review:

```
Review this change before I commit:
[paste your modified code]

Is anything risky? Any production data risk? Any test I should add? Any module dependency I forgot?
```

---

## Part 6: Things Copilot is BAD at (don't trust it on these)

| Topic | Risk | Always do |
|---|---|---|
| Database migrations | Can suggest data-destroying changes | Get senior approval |
| Security / access rules | Easy to leak data | Manual review by senior |
| Cron / scheduled jobs | Can hammer prod | Test on sandbox only |
| Email templates | Real emails could send | Verify outgoing mail server is dev only |
| Mass operations (write/unlink on many records) | Hard to undo | Test on small subset first |
| Custom SQL | Bypasses Odoo ORM safety | Avoid unless senior approves |
| External API keys | Copilot may hardcode them | Always use ir.config_parameter |
| Version-specific APIs | Copilot trained on mixed versions | Verify against Odoo official docs |

For any of these — write the code yourself based on Copilot's hints, don't paste blindly.

---

## Part 7: Useful follow-up prompts

After Copilot gives an answer, these one-liners often improve it:

- "Can you explain that to a junior developer in 3 sentences?"
- "What's the simplest possible version of this?"
- "What could go wrong with this in production?"
- "Show me a test case that would catch a bug in this code"
- "Is there a more idiomatic Odoo way to do this?"
- "What dependencies does this introduce?"
- "Is this Odoo [18/19] compatible? Verify the API exists."
- "Rewrite this without the [feature you don't understand]"

---

## Part 8: Quick reference — Copilot keyboard shortcuts (PyCharm Mac)

| Shortcut | Action |
|---|---|
| `Tab` | Accept inline suggestion |
| `Esc` | Reject inline suggestion |
| `Cmd+]` | Next suggestion |
| `Cmd+[` | Previous suggestion |
| `Cmd+Shift+I` | Open Copilot Chat |
| `Cmd+I` | Inline edit (select code first) |

---

## Part 9: When Copilot isn't enough

For these — search Odoo docs or ask senior:
- Migration scripts (data transformations, not just code)
- Custom security model (multi-company, record rules)
- Studio customizations
- Performance issues touching 100K+ records
- Anything mentioning "cron", "queue", or "celery"
- Multi-database setups
- Odoo.sh-specific deployment issues

Copilot doesn't know your specific Enconnex/Bytedance business logic — it knows Odoo generically. For business questions, ask your senior.

---

## Part 10: Save these prompts where you can find them

Copy this entire document into:
- A note in Apple Notes (locked)
- Or a file in your project: `~/Projects/copilot-prompts.md`
- Or print and keep next to monitor

Whenever you start a task: open this file, copy the matching template, customize, paste into Copilot Chat.

This is your "always-on senior helper" until you internalize Odoo patterns yourself (3-6 months).
