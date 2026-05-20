You're right. I've been guessing. Let me actually inspect the native Aged Receivable XLSX structure properly before writing more code.

## Diagnostic to capture both reports' EXACT structure (iTerm)

**Step 1:** Stop Odoo in PyCharm first (red ⏹). Then run:

```bash
cd ~/Projects/Enconnex && \
source .venv/bin/activate && \
echo "
import openpyxl, io

for report_xmlid in ['account_reports.aged_receivable_report',
                     'account_reports.aged_payable_report',
                     'account_reports.partner_ledger_report']:
    report = env.ref(report_xmlid)
    if 'aged' in report_xmlid:
        options = report.get_options({
            'date': {'mode': 'single', 'date_to': '2026-05-18', 'filter': 'today'},
            'unfold_all': True,
        })
    else:
        options = report.get_options({
            'date': {'mode': 'range', 'date_from': '2026-01-01',
                     'date_to': '2026-05-18', 'filter': 'custom'},
            'unfold_all': True,
        })
    options['unfold_all'] = True
    result = report.export_to_xlsx(options)
    file_bytes = result.get('file_content') if isinstance(result, dict) else None
    if not file_bytes:
        continue
    wb = openpyxl.load_workbook(io.BytesIO(file_bytes))
    ws = wb.active
    print(f'\n========== {report_xmlid} ==========')
    print(f'Sheet: {ws.title}, rows: {ws.max_row}, cols: {ws.max_column}')
    # Print first 30 rows to see structure including 2-3 partners
    for r in range(1, min(31, ws.max_row + 1)):
        row_vals = []
        for c in range(1, min(11, ws.max_column + 1)):
            v = ws.cell(row=r, column=c).value
            if v is None:
                row_vals.append('NULL')
            elif isinstance(v, str):
                row_vals.append(f'\"{v[:35]}\"')
            else:
                row_vals.append(str(v))
        print(f'R{r}: ' + ' | '.join(row_vals))
" | python3 odoo/odoo-bin shell -c odoo.conf -d enconnex_dev --no-http 2>&1 | grep -E "^(=|R[0-9]+:|Sheet)" > ~/diag_full.txt && \
echo "✅ Saved to ~/diag_full.txt"
```

**Run in:** iTerm

This will:
1. Generate native XLSX for all 3 reports
2. Print first 30 rows of each (enough to see 2-3 partners with their transactions)
3. Save to `~/diag_full.txt`
4. Filter out noise so file is small and readable

## Step 2: Open the file in PyCharm

In **PyCharm:**
1. File → Open → type `~/diag_full.txt` → Open
2. Select all (Cmd+A) → Copy (Cmd+C)
3. Paste into our chat here

## Why this is essential

Last time I only saw Aged Receivable with one partner. The bug must involve a specific row pattern (Enconnex-Rahi, MISC entries, etc.) that I haven't seen. Without the actual structure showing 2-3 partners with their transactions, I can't write correct detection logic.

## What I'll look for in the output

- Exact row structure where partner header sits
- Whether "MISC" appears as a journal code in col B for some rows
- Whether partner names with hyphens render differently
- How the row showing "Enconnex-Rahi" looks compared to "Accu-Tech"
- Any rows that don't follow the standard pattern

## Reply

Paste the content of `~/diag_full.txt` here as text.

Then I'll write a fix based on actual data, not guesses. No more random patches.
