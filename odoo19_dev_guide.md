# Odoo 19 Enterprise — Dev Workflow & Lessons Learned

**For:** Pankaj | **Updated:** after the MFG bug incident

---

## Part 1: Lessons learned (DO NOT REPEAT)

These are real mistakes from your past sessions. Avoid them.

### Mistake 1: Letting Copilot run shell commands
**What happened:** Copilot suggested `kill 77322`, `./odoo-bin -u all`, `CREATE EXTENSION vector`. Database got corrupted.

**Lesson:** Copilot should ONLY suggest code changes. If it suggests a shell command, you run it manually IF you understand it. Never click "Continue" or auto-run shell commands.

### Mistake 2: Letting Copilot fix "side errors"
**What happened:** You asked Copilot to fix MFG required field. Copilot also tried to fix `web_cohort`, `helpdesk.team`, `ai.embedding`. None of those were your problem.

**Lesson:** When Copilot says "let me also fix X, Y, Z" → say NO. Stay focused on the one specific task. Other errors in logs are background noise unless they block YOUR fix.

### Mistake 3: Using "Restore to Checkpoint"
**What happened:** This rolled back your good MFG fix AND Copilot's bad changes together.

**Lesson:** Never use Copilot's "restore to checkpoint" or "undo all" buttons. Use Git instead:
- `git status` — see what changed
- `git restore <file>` — undo unwanted file change
- `git diff <file>` — see the actual change

### Mistake 4: Editing `__manifest__.py` without thinking
**What happened:** Copilot added `web_cohort` as a dependency. This affects production when merged.

**Lesson:** `__manifest__.py` changes are HIGH RISK. Only edit if you genuinely need a new dependency, and ONLY with senior approval.

### Mistake 5: Token in screenshots
**What happened:** Twice you sent screenshots with your GitHub Personal Access Token visible.

**Lesson:** Before any Terminal screenshot:
1. Type `clear` to wipe visible screen
2. Run only what you want to show
3. Then screenshot
Or: paste text output instead of screenshots.

---

## Part 2: Daily dev workflow

### Morning startup (3 minutes)

```bash
# Step 1: Verify PostgreSQL is running
brew services list | grep postgres

# Step 2: Go to the project for today's task
cd ~/Projects/Bytedance      # or Enconnex

# Step 3: Pull latest from sandbox
cd custom
git checkout Sandbox-19      # or Sandbox-18 for Enconnex
git pull origin Sandbox-19

# Step 4: Start Odoo (or use PyCharm green ▶ button)
cd ..
source .venv/bin/activate
python odoo/odoo-bin -c odoo.conf -d bytedance_dev
```

### When you get a new task

```bash
# 1. Go to project
cd ~/Projects/Bytedance/custom

# 2. Make sure you're on sandbox + latest
git checkout Sandbox-19
git pull origin Sandbox-19

# 3. Create feature branch
git checkout -b feature/short-task-description

# 4. Make changes in PyCharm

# 5. Test locally — restart Odoo with -u module_name if model changed
```

### To restart Odoo after Python model change

In PyCharm:
1. Stop Odoo (red ⏹ button)
2. Edit Configurations → change Parameters to: `-c odoo.conf -d bytedance_dev -u module_name`
3. Run ▶
4. After testing, change Parameters back to: `-c odoo.conf -d bytedance_dev`

For XML view changes: just restart, no `-u` needed (already on `dev_mode=reload,qweb,xml` from odoo.conf).

### When change works — commit & push

```bash
cd ~/Projects/Bytedance/custom

# Verify what's changed
git status
git --no-pager diff

# Only commit if it shows what you expect
git add path/to/specific/file.py
git commit -m "Clear description of fix"
git push origin feature/short-task-description
```

### Open PR

1. Go to https://github.com/raghuchandra22/Bytedance_WMS
2. Click "Compare & pull request" yellow banner
3. ⚠️ Base branch: **Sandbox-19** (NOT master)
4. Title + description (what was broken, what you fixed)
5. Request senior review
6. Don't merge — wait for senior

### After PR merged — cleanup

```bash
cd ~/Projects/Bytedance/custom
git checkout Sandbox-19
git pull origin Sandbox-19
git branch -d feature/short-task-description    # delete local branch
```

---

## Part 3: How to fix a bug — proper workflow

### Step 1: Reproduce the bug
- Open Odoo locally
- Do the exact action that triggers the error
- Read the error message carefully
- Check terminal logs (in PyCharm bottom panel)

### Step 2: Locate the code

Use PyCharm:
- `Cmd + Shift + F` (Find in Files) — search for error keywords
- Search scope: limit to `./custom`
- Look at `.py` files (models) and `.xml` files (views)

### Step 3: Understand BEFORE changing
- Read the function or field definition
- Understand why it's the way it is
- Ask Copilot to EXPLAIN (don't ask to fix yet)

### Step 4: Plan the smallest change

Three categories from smallest to biggest:
- **Tiny:** add a default value, fix a typo
- **Small:** change a method, add a check
- **Big:** new field, new view, schema change → needs senior review FIRST

Start with the smallest possible fix that addresses the bug.

### Step 5: Make the change
- Edit ONE file
- Save (Cmd+S)
- Restart Odoo (with `-u module_name` if Python model)
- Test in browser

### Step 6: Verify nothing else broke
- Browse other features of the same module
- Check the logs for new errors
- If clean → commit

### Step 7: Commit + push + PR (see Part 2)

---

## Part 4: How to share bugs/issues with me (Claude) at home

When you encounter an issue at office and want my help that evening, share:

### Required (always)
1. **What you're trying to do** in plain English
2. **What error you see** — exact error message or screenshot
3. **Which file** has the relevant code — full path like `~/Projects/Bytedance/custom/ticket_management/models/helpdesk_ticket.py`
4. **The code** — paste the function or field that's relevant (10-30 lines max)

### Helpful (when applicable)
5. **Steps to reproduce** — "Go to Helpdesk → click New → fill X → click Save"
6. **What you've already tried**
7. **Odoo version** — 18 or 19 (just so I don't have to ask)
8. **Module name** — which custom module

### Don't share
- ❌ GitHub tokens (any token, password, API key)
- ❌ Customer data (real names, emails, financials)
- ❌ Database dumps
- ❌ Full screenshots of Terminal with command history visible

### Example good question

> "I'm trying to add a new field to helpdesk.ticket called 'priority_score'. I added it in `~/Projects/Bytedance/custom/ticket_management/models/helpdesk_ticket.py` like this:
>
> ```python
> priority_score = fields.Integer(string='Priority Score', compute='_compute_priority')
> ```
>
> But when I restart Odoo, I get this error:
> ```
> AttributeError: 'helpdesk.ticket' has no attribute '_compute_priority'
> ```
>
> I haven't added the compute method yet. What should I do?"

That's perfect. I can answer in 1 reply.

---

## Part 5: How to use Copilot SAFELY

### DO
- ✅ Place `copilot-instructions.md` in `.github/` folder of each project (provided separately)
- ✅ Give Copilot ONE specific task at a time
- ✅ Read explanations BEFORE looking at code
- ✅ Reject suggestions that touch multiple files
- ✅ Reject suggestions that include shell commands
- ✅ Use Copilot for: code completion, explanations, single-function changes

### DON'T
- ❌ Click "Continue" on auto-execute commands
- ❌ Click "Keep All" without reviewing the diff
- ❌ Let Copilot edit `__manifest__.py` unless you asked
- ❌ Use "Restore to Checkpoint" — use Git instead
- ❌ Ask Copilot to "fix everything" — too vague, dangerous
- ❌ Let Copilot run database/SQL commands

### Good Copilot prompts

```
"In file helpdesk_ticket.py, the mfg field is required but has no
default. Add default='mfg1'. Show only the lines to change. Risk level?"
```

```
"Explain what @api.depends does in this code. Don't change anything yet."
```

```
"In ticket_management/models/helpdesk_ticket.py, the create method
sometimes fails for empty values. Show me the create method (don't
change yet), then suggest a defensive check."
```

### Bad Copilot prompts (don't do this)

```
"Fix all the errors I see in the logs."
```
(Too broad, Copilot will go wild)

```
"Migrate this module to Odoo 19."
```
(Multi-file, schema, dependency changes — needs senior)

```
"Make Helpdesk work."
```
(Vague, multi-system)

---

## Part 6: Setup the `copilot-instructions.md` files (one-time)

For each project, create the file once. Copilot will read it automatically for every chat.

### For Bytedance

```bash
mkdir -p ~/Projects/Bytedance/.github
```

Then copy the content of `copilot-instructions-bytedance.md` to:
```
~/Projects/Bytedance/.github/copilot-instructions.md
```

### For Enconnex

```bash
mkdir -p ~/Projects/Enconnex/.github
```

Then copy the content of `copilot-instructions-enconnex.md` to:
```
~/Projects/Enconnex/.github/copilot-instructions.md
```

After that — every Copilot Chat in those projects will follow the rules. You'll get safer, more focused suggestions automatically.

---

## Part 7: Emergency recovery commands

When something goes wrong:

### Discard ALL changes (lose all current work)
```bash
cd ~/Projects/Bytedance/custom
git restore .
git clean -fd
```

### Discard changes to ONE file
```bash
git restore path/to/file.py
```

### See what changed
```bash
git status
git --no-pager diff
```

### Undo a commit (keep changes as unstaged)
```bash
git reset --soft HEAD~1
```

### Switch back to safe branch
```bash
git checkout Sandbox-19
```

### Reset local database (lose local data, restore from backup)
```bash
lsof -ti:8070 | xargs kill -9
dropdb -U odoo bytedance_dev
createdb -U odoo bytedance_dev
psql -U odoo -d bytedance_dev -f ~/Downloads/bytedance-restore/dump.sql
```

### Kill stuck Odoo
```bash
lsof -ti:8070 | xargs kill -9   # Bytedance
lsof -ti:8069 | xargs kill -9   # Enconnex
```

---

## Part 8: Production safety reminder

Your local Mac is **isolated**. Nothing you do locally affects production unless you:
1. Push to GitHub AND
2. PR gets merged to `master` (Bytedance) or `main` (Enconnex) AND
3. Odoo.sh auto-deploys to production

You control step 1 (push). Your senior controls step 2 (merge). Step 3 is automatic.

**As long as you don't push, you can break anything locally with zero impact.**

To verify nothing is pushed:
```bash
git log origin/master..HEAD --oneline   # Bytedance
git log origin/main..HEAD --oneline     # Enconnex
```
Empty output = production is safe.

---

## Final note

Your job for the next 3-6 months: deliver small, safe bug fixes. Don't try to be a hero with big refactors. Each small successful PR builds trust with your seniors. After 6 months, you'll naturally know what's safe to touch.

When in doubt — ASK ME (at home) or ASK SENIOR (at office). Better to ask than break production.
