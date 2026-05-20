Clear bug now that you described it. Let me explain root cause based on Odoo's structure.

## Root cause (short)

Looking at the Aged Receivable structure from our earlier diagnostic:

```
R4: "Aged Receivable" | NULL | totals...    ← report title (col B empty)
R5: "Accu-Tech"       | NULL | subtotals... ← partner header (col B empty)
R6: "INV/2026/.."     | "02/16/2026" | data ← invoice row (col B has date)
```

My code says: "If col B is NULL → partner header row."

**But:** A partner name with a hyphen or special character ("Enconnex-Rahi", "Enconnex B.V.") may render differently in the XLSX. Also, in Aged reports, the "MISC" or similar values can appear in the col B (Invoice Date) position when an entry doesn't have a real invoice date, making my code think it's a data row when it's actually a partner header.

The detection logic is too fragile. It assumes col B emptiness = partner header, but reality is messier.

**Better approach:** detect partner header rows by checking **multiple signals**:
- Col B (after insert = col 3) is NULL or empty
- Numeric columns to the right have values (subtotals)
- Text in col A doesn't match known subheaders (Initial Balance / Total / report titles)

Currently we only check col B emptiness. Need to also check that the row HAS numeric subtotals — that's the proof it's a partner-header row, not a stray malformed data row.

## Files changed

Only one file: `models/partner_ledger_flat_export.py`

## Exact command (run in iTerm)

```bash
cd ~/Projects/Enconnex/custom/enconnex_account_report && \
python3 << 'PYFIX'
import re
path = 'models/partner_ledger_flat_export.py'
with open(path) as f:
    content = f.read()

pattern = re.compile(
    r'    def _flatten_xlsx\(self, xlsx_bytes\):.*',
    re.DOTALL
)

new_block = '''    def _flatten_xlsx(self, xlsx_bytes):
        """Post-process Odoo's native XLSX to add Partner Name + Invoice
        column headers and flatten the structure.

        Detection rules (more robust than before):
        - Report titles ("Aged Receivable", "Partner Ledger" etc.) -> DELETE
        - Partner-header rows: col A has text + col B is empty + at least
          one numeric column to the right has a value (subtotals).
          -> capture partner name, DELETE row
        - Subheader rows ("Initial Balance", "Total"): col A matches
          known label -> KEEP, tag with current partner
        - Data rows: col B has value (Journal code or Invoice Date) ->
          KEEP, tag with current partner

        This works for Partner Ledger, Aged Receivable, Aged Payable.
        """
        from openpyxl.styles import Font, PatternFill, Alignment
        wb = openpyxl.load_workbook(io.BytesIO(xlsx_bytes))

        REPORT_TITLES = {
            'aged receivable', 'aged payable', 'partner ledger',
        }
        SUBHEADERS = {
            'initial balance', 'total', 'unaffected earnings',
        }

        for ws in wb.worksheets:
            min_row = ws.min_row
            max_row = ws.max_row

            ws.insert_cols(1)

            # Find header row by checking col 3 (was original col B) for
            # known column-header labels
            header_row = None
            for r in range(min_row, min(min_row + 8, max_row + 1)):
                cell_c = ws.cell(row=r, column=3).value
                if cell_c and isinstance(cell_c, str) and cell_c.strip() in (
                    'Journal', 'Account', 'Invoice Date', 'At Date',
                    'Reference',
                ):
                    header_row = r
                    break

            if header_row:
                # New column A header
                hc = ws.cell(row=header_row, column=1, value='Partner Name')
                hc.font = Font(bold=True)
                hc.fill = PatternFill(start_color='D9E1F2',
                                      end_color='D9E1F2',
                                      fill_type='solid')
                hc.alignment = Alignment(horizontal='center')
                # Column B (was original col A) header = Invoice/Description
                ic = ws.cell(row=header_row, column=2, value='Invoice')
                ic.font = Font(bold=True)
                ic.fill = PatternFill(start_color='D9E1F2',
                                      end_color='D9E1F2',
                                      fill_type='solid')
                ic.alignment = Alignment(horizontal='center')

            current_partner = None
            rows_to_delete = []
            data_start = (header_row or min_row) + 1
            max_col = ws.max_column

            for r in range(data_start, max_row + 1):
                # After insert_cols(1):
                # col 2 = original col A (Partner / Initial Balance / move name)
                # col 3 = original col B (Journal code / Invoice Date / NULL)
                label = ws.cell(row=r, column=2).value
                second = ws.cell(row=r, column=3).value

                if label is None:
                    continue
                if not isinstance(label, str):
                    if current_partner:
                        ws.cell(row=r, column=1, value=current_partner)
                    continue

                stripped = label.strip()
                if not stripped:
                    continue
                lower = stripped.lower()

                # Report title -> delete
                if lower in REPORT_TITLES:
                    rows_to_delete.append(r)
                    continue

                # Known subheader -> keep, tag
                if lower in SUBHEADERS:
                    if current_partner:
                        ws.cell(row=r, column=1, value=current_partner)
                    continue

                # Is "second" (col 3) empty?
                is_empty_second = (
                    second is None
                    or (isinstance(second, str) and not second.strip())
                )

                if is_empty_second:
                    # Looks like partner-header candidate. Verify by checking
                    # if any numeric column to the right has a value.
                    has_numbers = False
                    for c in range(4, max_col + 1):
                        v = ws.cell(row=r, column=c).value
                        if isinstance(v, (int, float)) and v != 0:
                            has_numbers = True
                            break
                    if has_numbers:
                        # Confirmed: partner-header row
                        current_partner = stripped
                        rows_to_delete.append(r)
                    else:
                        # Looks like a partner header but has no totals.
                        # Could be a malformed row or section divider.
                        # Capture partner name anyway but DON'T delete - safer.
                        current_partner = stripped
                else:
                    # Data row -> tag with partner
                    if current_partner:
                        ws.cell(row=r, column=1, value=current_partner)

            for r in sorted(rows_to_delete, reverse=True):
                ws.delete_rows(r, 1)

            ws.column_dimensions['A'].width = 35

        out = io.BytesIO()
        wb.save(out)
        out.seek(0)
        return out.read()
'''

content = pattern.sub(new_block, content)
with open(path, 'w') as f:
    f.write(content)
print("OK: partner detection now requires numeric subtotals to confirm partner-header row")
PYFIX
echo "Done. Now in PyCharm: stop Odoo, restart with -u enconnex_account_report"
```

**Run in:** iTerm

## What the fix does

Before: "col B empty → partner header." Wrong for special characters and weird data.

After: "col B empty AND at least one numeric column has a value → partner header." Numeric subtotals are the reliable signal because Odoo always puts partner totals on partner-header rows.

## Simple test steps

After running the command:

1. **PyCharm:** Stop Odoo (red ⏹)
2. **PyCharm:** Edit Configurations → Parameters: `-c odoo.conf -d enconnex_dev -u enconnex_account_report`
3. **PyCharm:** Click green ▶
4. Wait for `HTTP service running on ...:8069`
5. **Browser:** http://localhost:8069 → Accounting → Reporting → Partner Reports → Exports
6. Select **Aged Receivable** → Generate
7. Open downloaded Excel → look for "Enconnex-Rahi" rows:
   - Column A should now show **"Enconnex-Rahi US Vendor-Intercompany"** (or similar full name)
   - Column B (Invoice) should show the actual invoice ID, NOT "MISC..."
8. Repeat for **Aged Payable** and **Partner Ledger** to confirm no regression

## Reply

- ✅ All 3 reports show correct partner names → I'll move us to Step 2 (create the standalone module zip for senior)
- ❌ Still wrong → tell me which specific name + what column A and column B show
