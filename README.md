Empty output means the script ran but the filter dropped everything (or the script errored before producing rows). Let me get raw output without filtering.

## Run this in iTerm

```bash
cd ~/Projects/Enconnex && \
source .venv/bin/activate && \
echo "
import openpyxl, io
for report_xmlid in ['account_reports.aged_receivable_report',
                     'account_reports.aged_payable_report']:
    print('========== ' + report_xmlid + ' ==========')
    report = env.ref(report_xmlid)
    options = report.get_options({
        'date': {'mode': 'single', 'date_to': '2026-05-18', 'filter': 'today'},
        'unfold_all': True,
    })
    options['unfold_all'] = True
    result = report.export_to_xlsx(options)
    file_bytes = result.get('file_content') if isinstance(result, dict) else None
    if not file_bytes:
        print('NO FILE BYTES')
        continue
    wb = openpyxl.load_workbook(io.BytesIO(file_bytes))
    ws = wb.active
    print('Sheet: ' + str(ws.title) + ' rows: ' + str(ws.max_row))
    for r in range(1, min(31, ws.max_row + 1)):
        parts = []
        for c in range(1, min(11, ws.max_column + 1)):
            v = ws.cell(row=r, column=c).value
            if v is None:
                parts.append('NULL')
            elif isinstance(v, str):
                parts.append(repr(v[:35]))
            else:
                parts.append(str(v))
        print('R' + str(r) + ': ' + ' | '.join(parts))
" | python3 odoo/odoo-bin shell -c odoo.conf -d enconnex_dev --no-http > ~/diag_full.txt 2>&1 && \
echo "✅ DONE" && \
echo "File size:" && \
wc -l ~/diag_full.txt && \
echo "" && \
echo "First 5 lines (sanity check):" && \
head -5 ~/diag_full.txt && \
echo "" && \
echo "Last 5 lines:" && \
tail -5 ~/diag_full.txt
```

**Run in:** iTerm (make sure Odoo is stopped first in PyCharm)

## What's different

- No `grep -E` filter — captures EVERYTHING
- Shows file size + first/last 5 lines so we know if it has content
- Saves to `~/diag_full.txt`

## Reply

Paste the output of the command above (file size + first/last lines).

If file has content (>50 lines) → open `~/diag_full.txt` in PyCharm, copy whole thing, paste here.

If file is small or shows error → paste what's there, I'll diagnose.

If "odoo-bin shell" hangs or fails → tell me, we'll try a different approach.
