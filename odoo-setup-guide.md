# Odoo Local Setup Guide — Office Mac

**For:** Pankaj | **Projects:** Enconnex (Odoo 18) and Bytedance (Odoo 19)
**Your Mac:** macOS 26 on Apple Silicon

---

## How to use this guide

1. Open **Terminal** (Cmd+Space → type "Terminal" → Enter)
2. Copy each command block and paste into Terminal, press Enter
3. Wait for it to finish before running the next one
4. If something fails, jump to **Section 8: Common Errors** at the bottom
5. Do the steps **in order**. Don't skip ahead.

⚠️ Replace `YOUR-ORG` and repo URLs with the real GitHub URLs from your team. Ask your senior if unsure.

---

## What you already have ✅

Already installed on your Mac (don't reinstall):
- Homebrew (at `/opt/homebrew/bin/brew`)
- Python 3.11, 3.12, 3.13
- Git
- Node.js + npm
- PyCharm

What needs fixing:
- ❌ PostgreSQL is broken — we'll fix it in Step 1

---

## Step 1: Fix PostgreSQL

Your PostgreSQL is in error state. We'll uninstall the broken version and install a fresh one.

```bash
# Stop the broken service
brew services stop postgresql@14

# Remove broken PostgreSQL 14
brew uninstall postgresql@14
rm -rf /opt/homebrew/var/postgresql@14
```

```bash
# Install fresh PostgreSQL 15
brew install postgresql@15
```

```bash
# Add PostgreSQL 15 to your PATH (so commands work)
echo 'export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

```bash
# Start PostgreSQL
brew services start postgresql@15
```

```bash
# Wait 5 seconds, then check it's running
sleep 5 && brew services list | grep postgres
```

You should see `postgresql@15 started`. If you see `error`, go to Section 8.

```bash
# Create a database user named 'odoo' (Odoo will use this)
createuser -s odoo
```

```bash
# Verify everything works
psql -U odoo -d postgres -c "SELECT version();"
```

You should see PostgreSQL version info. ✅ Done with Step 1.

---

## Step 2: Create your folder structure

```bash
# Create main projects folder
mkdir -p ~/Projects/Enconnex
mkdir -p ~/Projects/Bytedance
cd ~/Projects
ls
```

You should see `Enconnex` and `Bytedance` listed.

---

## Step 3: Set up Enconnex (Odoo 18)

### 3.1 Clone the code

```bash
cd ~/Projects/Enconnex
```

```bash
# Clone official Odoo 18 source (this takes 3-5 minutes)
git clone --depth 1 --branch 18.0 https://github.com/odoo/odoo.git odoo
```

```bash
# Clone the Enconnex client repo (REPLACE THE URL with your real one)
git clone https://github.com/YOUR-ORG/enconnex.git custom
```

When prompted for GitHub login, use your username and a **Personal Access Token** (not password). Get one at github.com → Settings → Developer settings → Personal access tokens → Generate new token (classic) → tick `repo` → copy and save it.

### 3.2 Create the Python virtual environment

A "virtual environment" is just a private Python folder for this project — keeps things isolated.

```bash
cd ~/Projects/Enconnex
python3.11 -m venv .venv
```

```bash
# Activate it (you'll see (.venv) appear in your terminal prompt)
source .venv/bin/activate
```

```bash
# Upgrade pip
pip install --upgrade pip wheel setuptools
```

### 3.3 Install Apple Silicon dependencies

These are needed for Odoo's Python packages to compile on M1/M2/M3 Macs:

```bash
brew install libpq libxml2 libxslt libjpeg openssl@3 zlib freetype
```

```bash
# Tell pip where to find them
export LDFLAGS="-L/opt/homebrew/opt/libpq/lib -L/opt/homebrew/opt/openssl@3/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libpq/include -I/opt/homebrew/opt/openssl@3/include"
```

### 3.4 Install Odoo's Python requirements

```bash
cd ~/Projects/Enconnex
pip install -r odoo/requirements.txt
```

⏱ This takes 5-10 minutes. If it fails, see Section 8.

### 3.5 Install wkhtmltopdf (for PDF reports)

```bash
brew install --cask wkhtmltopdf
```

If brew says "no longer available", skip this — Odoo will work without it (you just can't print PDF reports locally).

### 3.6 Create the database

```bash
createdb -U odoo enconnex_dev
```

```bash
# Verify it exists
psql -U odoo -l | grep enconnex
```

### 3.7 Create odoo.conf (the settings file)

```bash
cat > ~/Projects/Enconnex/odoo.conf << 'EOF'
[options]
addons_path = ./odoo/addons,./custom
db_host = localhost
db_port = 5432
db_user = odoo
db_password = False
db_name = enconnex_dev
http_port = 8069
admin_passwd = admin
log_level = info
dev_mode = reload,qweb,xml
EOF
```

⚠️ **If your custom modules are inside a subfolder** (like `custom/addons/` instead of directly in `custom/`), open the file in PyCharm and change `./custom` to `./custom/addons`. Check by running:

```bash
ls ~/Projects/Enconnex/custom
```

If you see folders with `__manifest__.py` files inside them directly, the path is correct as `./custom`. If you see another folder like `addons` first, change it to `./custom/addons`.

### 3.8 Run Odoo from Terminal (first time)

```bash
cd ~/Projects/Enconnex
source .venv/bin/activate
python odoo/odoo-bin -c odoo.conf -d enconnex_dev -i base
```

Watch the output. When you see a line like `HTTP service (werkzeug) running on ... port 8069`, open your browser:

👉 **http://localhost:8069**

Login with:
- Email: `admin`
- Password: `admin`

🎉 You're inside Odoo! Press `Ctrl+C` in Terminal to stop it when done.

### 3.9 Set up PyCharm for Enconnex (so you can run from PyCharm)

1. Open PyCharm → **File → Open** → select `~/Projects/Enconnex` → Open
2. Bottom-right of PyCharm shows Python interpreter. Click it → **Add New Interpreter → Add Local Interpreter → Existing**
3. Browse to: `~/Projects/Enconnex/.venv/bin/python` → OK
4. Top-right of PyCharm: **Add Configuration → + → Python**
5. Fill in:
   - **Name:** `Enconnex Odoo 18`
   - **Script path:** click folder icon → `~/Projects/Enconnex/odoo/odoo-bin`
   - **Parameters:** `-c odoo.conf -d enconnex_dev`
   - **Working directory:** `~/Projects/Enconnex`
   - **Python interpreter:** the .venv one you just added
6. Click OK
7. Click the green ▶ Play button to run

---

## Step 4: Set up Bytedance (Odoo 19)

Same as Step 3, but with these changes:

```bash
cd ~/Projects/Bytedance
```

```bash
# Clone Odoo 19
git clone --depth 1 --branch 19.0 https://github.com/odoo/odoo.git odoo
```

```bash
# Clone Bytedance custom repo (REPLACE URL)
git clone https://github.com/YOUR-ORG/bytedance.git custom
```

```bash
# Use Python 3.12 for Odoo 19
python3.12 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip wheel setuptools
```

```bash
export LDFLAGS="-L/opt/homebrew/opt/libpq/lib -L/opt/homebrew/opt/openssl@3/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libpq/include -I/opt/homebrew/opt/openssl@3/include"
pip install -r odoo/requirements.txt
```

```bash
createdb -U odoo bytedance_dev
```

Create `odoo.conf` with **port 8070** (different from Enconnex so they don't clash):

```bash
cat > ~/Projects/Bytedance/odoo.conf << 'EOF'
[options]
addons_path = ./odoo/addons,./custom
db_host = localhost
db_port = 5432
db_user = odoo
db_password = False
db_name = bytedance_dev
http_port = 8070
admin_passwd = admin
log_level = info
dev_mode = reload,qweb,xml
EOF
```

Run it:

```bash
cd ~/Projects/Bytedance
source .venv/bin/activate
python odoo/odoo-bin -c odoo.conf -d bytedance_dev -i base
```

Open: **http://localhost:8070**

In PyCharm, repeat Step 3.9 but for the Bytedance folder.

---

## Step 5: Understanding the project structure

Your folder looks like:

```
~/Projects/Enconnex/
├── .venv/              ← Python virtual environment (don't touch)
├── odoo/               ← Official Odoo source (don't edit this)
│   ├── addons/         ← Standard Odoo modules
│   └── odoo-bin        ← The program you run
├── custom/             ← Client's custom modules (you edit HERE)
│   ├── module_one/
│   ├── module_two/
│   └── ...
└── odoo.conf           ← Your local settings
```

**Inside any custom module:**

```
module_name/
├── __init__.py         ← Lists Python files to load
├── __manifest__.py     ← Module info: name, version, dependencies
├── models/             ← Python files (database tables & business logic)
├── views/              ← XML files (forms, lists, menus)
├── security/           ← Access rules (who can see/edit what)
├── data/               ← Default data loaded on install
├── static/             ← Images, JS, CSS
├── reports/            ← PDF report templates
└── wizards/            ← Pop-up dialogs
```

**Where to look first** when fixing a bug:
1. `__manifest__.py` — what does this module do, what depends on it
2. `models/*.py` — the logic
3. `views/*.xml` — the UI

---

## Step 6: Daily workflow (your normal day)

### Morning: pull the latest code

```bash
cd ~/Projects/Enconnex/custom
git checkout staging       # or whatever your team's main branch is called
git pull origin staging
```

### Make a change

```bash
# Create a branch for your work
git checkout -b fix-invoice-bug
```

Edit code in PyCharm. Save files.

### Test it locally

In PyCharm, stop Odoo (red square button). Restart with module update:

Edit your Run Configuration → change Parameters to:
```
-c odoo.conf -d enconnex_dev -u module_name
```

(`-u` means "update this module"). Run again. Test in browser.

When done testing, change parameters back to `-c odoo.conf -d enconnex_dev` for normal runs.

### Commit and push

```bash
cd ~/Projects/Enconnex/custom
git add .
git status                              # check what you're committing
git commit -m "Fix invoice tax calculation on POS receipts"
git push origin fix-invoice-bug
```

### Open a Pull Request

1. Go to GitHub in browser
2. You'll see a yellow banner: "Compare & pull request" → click it
3. Set base branch to `staging` (NOT `production`/`main`)
4. Write a clear title and description
5. Request review from your senior
6. Wait for approval, then merge

Once merged into `staging`, **Odoo.sh automatically deploys** to the sandbox environment. Test there with realistic data.

After client/senior approves on sandbox: merge `staging` → `production` → Odoo.sh deploys to live system.

### The full safe flow

```
Your Mac (local test)
    ↓ git push
GitHub (your branch)
    ↓ Pull Request → merge into staging
Odoo.sh sandbox (auto-deployed)
    ↓ test with real data, get approval
Odoo.sh production (merge staging → production)
    ↓
LIVE CLIENT SYSTEM
```

---

## Step 7: Best practices (read this twice)

✅ **Always do:**
- Pull before starting work
- One feature/fix per branch
- Test locally before pushing
- Test on sandbox before production
- Write clear commit messages
- Keep Odoo 18 and 19 in completely separate folders/venvs

❌ **Never do:**
- Edit files inside `odoo/` folder (that's the official Odoo source — extend it via custom modules instead)
- Push directly to `production` or `main` branch
- Commit `odoo.conf` with passwords (it's safe locally because it has none)
- Mix Odoo 18 and 19 in the same virtualenv
- Drop or modify the production database directly
- Run `git push --force` on shared branches

🆘 **If you break production:**
- Don't panic
- Tell your senior immediately
- Odoo.sh has a **"Revert to previous build"** button — find it in the Odoo.sh interface BEFORE you ever need it

---

## Step 8: Common errors and fixes

### Error: `psycopg2` install fails

```bash
brew install libpq
export LDFLAGS="-L/opt/homebrew/opt/libpq/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libpq/include"
pip install psycopg2-binary
```

### Error: `lxml` install fails

```bash
brew install libxml2 libxslt
export LDFLAGS="-L/opt/homebrew/opt/libxml2/lib -L/opt/homebrew/opt/libxslt/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libxml2/include -I/opt/homebrew/opt/libxslt/include"
pip install lxml
```

### Error: `Pillow` install fails

```bash
brew install libjpeg zlib freetype
pip install Pillow
```

### Error: `port 8069 already in use`

Another Odoo is still running. Find and kill it:
```bash
lsof -ti:8069 | xargs kill -9
```

Or change `http_port` in `odoo.conf` to `8071` and try again.

### Error: `could not connect to server: Connection refused`

PostgreSQL is not running.
```bash
brew services start postgresql@15
sleep 3
brew services list | grep postgres
```

### Error: `database "xxx_dev" does not exist`

```bash
createdb -U odoo enconnex_dev      # or bytedance_dev
```

### Error: `role "odoo" does not exist`

```bash
createuser -s odoo
```

### Error: `Module xxx not found` when starting Odoo

Your `addons_path` in `odoo.conf` is wrong. Check what's actually inside `custom/`:
```bash
ls ~/Projects/Enconnex/custom
```

If you see folders containing `__manifest__.py` directly, path is `./custom`.
If folders are nested deeper, like `custom/addons/`, path is `./custom/addons`.

### Error: Brew service shows `error` for postgresql

```bash
# View the error log
tail -50 /opt/homebrew/var/log/postgresql@15.log

# Nuclear option: full reset
brew services stop postgresql@15
brew uninstall postgresql@15
rm -rf /opt/homebrew/var/postgresql@15
brew install postgresql@15
brew services start postgresql@15
createuser -s odoo
```

### Error: `git push` says "Permission denied"

You need a Personal Access Token from GitHub:
1. github.com → top-right avatar → Settings
2. Developer settings (left sidebar, very bottom)
3. Personal access tokens → Tokens (classic) → Generate new token
4. Tick `repo`, set expiration, generate, copy
5. When git asks for password, paste this token

### Error: Changes don't show up after editing

Restart Odoo with `-u module_name` parameter. For view (XML) changes, also turn on Developer Mode in Odoo (Settings → Developer Tools).

### Error: `pg_config executable not found`

```bash
echo 'export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

## Step 9: Quick reference cheat sheet

### Start your day
```bash
brew services list | grep postgres   # make sure postgres is up
cd ~/Projects/Enconnex
source .venv/bin/activate
git pull origin staging
```

### Run Enconnex
```bash
cd ~/Projects/Enconnex
source .venv/bin/activate
python odoo/odoo-bin -c odoo.conf -d enconnex_dev
```
Browser: http://localhost:8069

### Run Bytedance
```bash
cd ~/Projects/Bytedance
source .venv/bin/activate
python odoo/odoo-bin -c odoo.conf -d bytedance_dev
```
Browser: http://localhost:8070

### Update a module after editing
Add `-u module_name` to the run command, or use `--dev=reload` for auto-reload of Python changes.

### Reset database (start fresh)
```bash
dropdb -U odoo enconnex_dev
createdb -U odoo enconnex_dev
# then run Odoo with -i base to re-initialize
```

### Stop Odoo
Press `Ctrl+C` in the Terminal where it's running.

### Stop PostgreSQL
```bash
brew services stop postgresql@15
```

---

## Final tips

- **Keep a personal notes file** of every weird issue you fix and how. You'll see them again.
- **Read commit messages** of past work in `git log` to learn how your team writes code.
- **When stuck**, search the exact error message on Odoo's forum (odoo.com/forum) — someone has hit it before.
- **Ask questions early.** Better to ask your senior than to break production.

You've got this. 💪

---

*Last updated for: Odoo 18 (Enconnex), Odoo 19 (Bytedance), macOS Apple Silicon, PostgreSQL 15*
