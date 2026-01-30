# Odoo 19 Migration - Comprehensive Testing Checklist

## Pre-Testing Setup

### Environment Preparation
- [ ] Fresh Odoo 19 instance installed (Enterprise or Community as needed)
- [ ] Database backup created before testing
- [ ] Test data prepared (products, partners, stock locations, etc.)
- [ ] All module ZIPs extracted to addons path
- [ ] `openpyxl` Python package installed (`pip install openpyxl`)
- [ ] Module path added to odoo.conf

### Initial Verification
- [ ] All modules appear in Apps list (Update Apps List first)
- [ ] No Python syntax errors on Odoo startup
- [ ] Log files checked for warnings/errors

---

## Batch 1 Modules

---

### 1. stock_report_cn (Stock Reports - Chinese)

**Module Info:** Inventory receipt/dispatch reports with quantity and value tracking

#### Installation Test
- [ ] Module installs without errors
- [ ] All menu items appear under Inventory/Reporting
- [ ] No JavaScript console errors

#### Functional Tests

**1.1 Stock Detail Report (收发存报表-数量)**
- [ ] Navigate to: Inventory → Reporting → Stock Detail Report
- [ ] Wizard opens correctly
- [ ] Date range filter works
- [ ] Location filter works
- [ ] Product category filter works
- [ ] Report generates without errors
- [ ] Data displays correctly (source, product, qty, date)
- [ ] Export to Excel works (if available)

**1.2 Stock Total Report (收发存报表-金额)**
- [ ] Navigate to: Inventory → Reporting → Stock Total Report
- [ ] Wizard opens correctly
- [ ] Report shows quantity AND value columns
- [ ] Values calculate correctly (unit price × quantity)
- [ ] Grouping by product/category works

**1.3 Stock Age Report (账龄表)**
- [ ] Report generates based on stock quant in_date
- [ ] Age buckets display correctly (days)
- [ ] Aging calculation is accurate

**1.4 Stock Turnover Report (库存周转率)**
- [ ] Turnover days calculate correctly
- [ ] Turnover rate displays properly
- [ ] Based on Stock Valuation Layer data

**1.5 Stock Sale Forecast Report (库存出库预测)**
- [ ] Daily average outbound quantity calculates
- [ ] Available days for on-hand stock calculates
- [ ] Forecast logic is accurate

#### Data Integrity Tests
- [ ] Create stock move → Verify appears in detail report
- [ ] Complete transfer → Verify value updates in total report
- [ ] Reports match actual stock.move records

---

### 2. wesco_serial (Serial/Lot Tracking Enhancements)

**Module Info:** Extended serial number management with Excel import

#### Installation Test
- [ ] Module installs without errors
- [ ] Dependencies resolved (purchase_stock, mrp, stock_report_cn)
- [ ] Views load without XML errors

#### Functional Tests

**2.1 Purchase Order Lot Lines**
- [ ] Navigate to: Purchase → Orders → Purchase Orders
- [ ] Create new PO with tracked product
- [ ] Lot line fields visible on PO lines
- [ ] Can pre-define lot/serial numbers on PO

**2.2 Stock Picking Excel Import**
- [ ] Open a receipt (incoming picking)
- [ ] "Upload Excel" button visible when state = assigned/confirmed
- [ ] Button hidden in other states
- [ ] Prepare Excel file with columns: lot_name, location_id, pallet_name
- [ ] Upload Excel file
- [ ] Move lines created correctly from Excel data
- [ ] Error handling works for:
  - [ ] Missing lots
  - [ ] Invalid locations
  - [ ] Malformed Excel

**2.3 MRP Production Excel Import**
- [ ] Open Manufacturing Order
- [ ] "Upload Excel" button visible in confirmed/progress/to_close states
- [ ] Import works for raw material consumption
- [ ] Serial numbers validate against existing stock

**2.4 Stock Move Line Enhancements**
- [ ] Pallet name field visible on move lines
- [ ] Location tracking works correctly

**2.5 Serial Number Validation**
- [ ] System prevents duplicate serial receipt
- [ ] Error message displays: "SN has been in stock: [serial]"

#### Edge Cases
- [ ] Import with blank rows in Excel → Skips gracefully
- [ ] Import with missing columns → Clear error message
- [ ] Large Excel file (1000+ rows) → Performance acceptable

---

### 3. ks_dashboard_ninja (Dashboard Ninja)

**Module Info:** Dynamic dashboard builder with charts, KPIs, tiles

#### ⚠️ Pre-Test Warning
This module requires JavaScript migration for full functionality. Test basic features first.

#### Installation Test
- [ ] Module installs without errors
- [ ] Dashboard menu appears
- [ ] No JavaScript console errors (check browser DevTools)

#### Functional Tests (Python/Backend)

**3.1 Dashboard CRUD**
- [ ] Create new dashboard
- [ ] Name field works (35 char limit)
- [ ] Menu name configurable
- [ ] Top menu selection works
- [ ] Group access restriction works
- [ ] Dashboard saves successfully

**3.2 Dashboard Items (Backend)**
- [ ] Create KPI item via form view
- [ ] Create Tile item via form view
- [ ] Model selection works
- [ ] Domain filter configurable
- [ ] Date filter selection works
- [ ] Record count type (count/sum/avg) works

**3.3 Date Filters**
- [ ] "All Time" filter works
- [ ] "Today" filter works
- [ ] "This Week/Month/Quarter/Year" filters work
- [ ] "Last 7/30/90/365 days" filters work
- [ ] Custom date range works

**3.4 Data Formatting**
- [ ] Global format works
- [ ] Indian format works
- [ ] Exact format works

#### JavaScript-Dependent Features (May Fail)
- [ ] Dashboard renders in browser
- [ ] Gridstack layout works (drag/drop items)
- [ ] Charts render (bar, line, pie, etc.)
- [ ] Real-time refresh works
- [ ] Export to PDF works
- [ ] Color picker works

#### Known Issues to Document
- [ ] Note any JS console errors
- [ ] Document which features work/don't work
- [ ] Screenshot error states

---

### 4. ticket_management (Helpdesk Extension)

**Module Info:** TikTok/Bytedance issue reporting extension for Helpdesk

#### ⚠️ Pre-Test Requirement
- Requires Odoo Enterprise (helpdesk, website_sale_dashboard modules)

#### Installation Test
- [ ] Module installs without errors (Enterprise only)
- [ ] Helpdesk menu accessible
- [ ] Custom fields visible on ticket form

#### Functional Tests

**4.1 Ticket Creation**
- [ ] Navigate to: Helpdesk → Tickets
- [ ] Create new ticket
- [ ] Custom fields visible:
  - [ ] Note (readonly, default text displays)
  - [ ] Report Date (required, defaults to today)
  - [ ] MFG selection (MFG 1, MFG 2)
  - [ ] Model selection
  - [ ] Issue selection
  - [ ] Issue Verification (attachment field)
  - [ ] Action Items (required)
  - [ ] Status selection
  - [ ] Close Date
  - [ ] Solution
  - [ ] Reported To (employee selection, required)

**4.2 Ticket Workflow**
- [ ] Ticket saves successfully
- [ ] Required field validation works
- [ ] Employee lookup works for "Reported To"

---

## Batch 2 Modules

---

### 5. app_common (Base Utilities)

**Module Info:** Common utilities for Sunpop apps (user agent parsing, cron enhancements)

#### Installation Test
- [ ] Module installs without errors
- [ ] No missing dependencies

#### Functional Tests

**5.1 Base Model Extensions**
- [ ] Check if get_ua_info method available on models
- [ ] User agent parsing works (test via controller)

**5.2 Cron Enhancements**
- [ ] Navigate to: Settings → Technical → Scheduled Actions
- [ ] Enhanced views load correctly
- [ ] Cron jobs execute properly

**5.3 Mail Server Enhancements**
- [ ] Navigate to: Settings → Technical → Outgoing Mail Servers
- [ ] Extended fields visible (if any)

---

### 6. app_odoo_customize (Odoo Customization Suite)

**Module Info:** Comprehensive Odoo UI customization (branding, ribbon, menu modifications)

#### Installation Test
- [ ] Module installs without errors
- [ ] Settings menu accessible

#### Functional Tests

**6.1 General Settings**
- [ ] Navigate to: Settings → General Settings
- [ ] "Odoo Customize" section visible
- [ ] All toggle options save correctly

**6.2 Branding Options**
- [ ] Company logo changes reflect in UI
- [ ] Favicon customization works
- [ ] Login page customization works

**6.3 Environment Ribbon**
- [ ] Development/Staging ribbon displays (if enabled)
- [ ] Ribbon color customizable
- [ ] Ribbon text customizable

**6.4 Menu Customization**
- [ ] Menu hiding works
- [ ] Menu reordering works
- [ ] Documentation link customization works

**6.5 User Menu**
- [ ] User menu dropdown works
- [ ] Custom menu items appear
- [ ] Language switcher works (if enabled)

**6.6 Module List Enhancements**
- [ ] Navigate to: Apps
- [ ] Version comparison visible
- [ ] Install version displays
- [ ] Local updatable filter works

**6.7 Dialog Customization**
- [ ] Modal dialogs display correctly
- [ ] Custom styling applies

---

### 7. bytedance_odoo_connector (Bytedance/TikTok WMS Integration)

**Module Info:** Integration between Odoo and Bytedance WMS system

#### Installation Test
- [ ] Module installs without errors
- [ ] All menus appear
- [ ] Views load without XML errors

#### Functional Tests

**7.1 Bytedance Instance Configuration**
- [ ] Navigate to: Bytedance → Configuration → Instances
- [ ] Create new instance
- [ ] Activate/Deactivate toggle works
- [ ] Connection settings save

**7.2 Product Integration**
- [ ] Navigate to: Inventory → Products
- [ ] Bytedance fields visible on product form
- [ ] BD SKU number field works
- [ ] BD category field works

**7.3 Purchase Order Integration**
- [ ] Navigate to: Purchase → Orders
- [ ] Bytedance tab visible on PO form
- [ ] BD fields populate correctly
- [ ] Serial number management works
- [ ] Accept/Reject serial functionality works

**7.4 Stock Picking Integration**
- [ ] Bytedance info tab visible on pickings
- [ ] Warehouse number field works
- [ ] Business number field works
- [ ] "Send WMS Feedback" button visible (done state)
- [ ] Feedback sends correctly

**7.5 MRP Workorder Integration**
- [ ] Bytedance fields visible on workorders
- [ ] Serial acceptance workflow works

**7.6 Sale Order Integration**
- [ ] BD Code field visible
- [ ] Plan work time integration works

**7.7 Logging**
- [ ] Navigate to: Bytedance → Logs
- [ ] API calls logged correctly
- [ ] Errors captured with details

**7.8 Stock Quant/Lot Integration**
- [ ] Extended fields visible on lots
- [ ] Active/Inactive toggle works

#### API Tests (If Bytedance Sandbox Available)
- [ ] Test connection to Bytedance API
- [ ] Push order test
- [ ] Receive feedback test
- [ ] Error handling for failed calls

---

### 8. deltatech_merge (Record Merge Utility)

**Module Info:** Generic record merging wizard

#### Installation Test
- [ ] Module installs without errors
- [ ] Wizard accessible

#### Functional Tests

**8.1 Merge Wizard Access**
- [ ] Wizard can be invoked on partner records
- [ ] Wizard opens correctly

**8.2 Merge Functionality**
- [ ] Select multiple records
- [ ] Choose target record
- [ ] Preview shows merge changes
- [ ] Merge executes successfully
- [ ] Related records updated correctly
- [ ] Duplicate records archived/deleted

**8.3 Merge Validation**
- [ ] Cannot merge records of different types
- [ ] Proper error messages for invalid selections

---

### 9. deltatech_merge_product (Product Merge)

**Module Info:** Product-specific merge functionality

#### Installation Test
- [ ] Module installs without errors
- [ ] Depends on deltatech_merge resolved

#### Functional Tests

**9.1 Product Merge Access**
- [ ] Navigate to: Inventory → Products
- [ ] Select multiple products
- [ ] Action → Merge Products available

**9.2 Product Merge Execution**
- [ ] Select 2+ products to merge
- [ ] Choose destination product
- [ ] Preview shows affected records:
  - [ ] Stock moves
  - [ ] Purchase order lines
  - [ ] Sale order lines
  - [ ] Manufacturing orders
- [ ] Merge completes successfully
- [ ] Stock quantities consolidated
- [ ] History preserved on destination

**9.3 Merge Validation**
- [ ] Cannot merge products with different types
- [ ] Cannot merge products with different UoM categories
- [ ] Warning for products with different costs

---

### 10. eg_cancel_stock_move (Cancel Stock Moves)

**Module Info:** Allows cancellation of done stock moves

#### Installation Test
- [ ] Module installs without errors
- [ ] Views load correctly

#### Functional Tests

**10.1 Cancel Move Action**
- [ ] Navigate to: Inventory → Reporting → Stock Moves
- [ ] Find a completed (done) move
- [ ] Cancel action available
- [ ] Move cancels successfully
- [ ] Stock quantities revert correctly

**10.2 Cascade Effects**
- [ ] Related quants update
- [ ] Valuation layers update (if applicable)
- [ ] Picking status updates

**10.3 Validation**
- [ ] Cannot cancel moves with subsequent moves
- [ ] Error message for invalid cancellations

---

### 11. hspl_user_simulation (User Simulation)

**Module Info:** Simulate actions as another user for testing/support

#### ⚠️ Pre-Test Warning
JavaScript migration required for widget. Test backend functionality only.

#### Installation Test
- [ ] Module installs without errors
- [ ] Menu/Action accessible

#### Functional Tests

**11.1 Simulation Wizard**
- [ ] Navigate to: Settings → Users → Users
- [ ] Select user
- [ ] "Simulate User" action available
- [ ] Wizard opens (if JS works)

**11.2 Backend Simulation**
- [ ] Simulation record created
- [ ] User context switches
- [ ] Actions performed as target user
- [ ] Return to original user works

**11.3 Security**
- [ ] Only authorized users can simulate
- [ ] Cannot simulate admin users (safety)
- [ ] Audit log created for simulations

---

### 12. ks_dn_advance (Dashboard Ninja Advance)

**Module Info:** Advanced features for Dashboard Ninja (TV mode, query builder)

#### ⚠️ Pre-Test Requirement
- Requires ks_dashboard_ninja to be installed and functional

#### Installation Test
- [ ] Module installs without errors
- [ ] Dependencies resolved (ks_dashboard_ninja, mail)

#### Functional Tests

**12.1 Query-Based Items**
- [ ] Create dashboard item with query calculation type
- [ ] SQL query field visible
- [ ] Query executes and returns data
- [ ] X/Y labels configurable for query results

**12.2 Date Range Features**
- [ ] Date range toggle works
- [ ] Start/end date fields appear when enabled
- [ ] Date-based filtering works

**12.3 Enhanced Item Options**
- [ ] Additional chart options visible
- [ ] Grouped list view type works
- [ ] Ungrouped list view type works

**12.4 TV Dashboard Mode (JS-dependent)**
- [ ] TV dashboard menu accessible
- [ ] Full-screen mode works
- [ ] Auto-rotation between items works
- [ ] Website dashboard integration works

**12.5 Mail Integration**
- [ ] Dashboard can be shared via email
- [ ] Email template works

---

### 13. mass_cancel_purchase_order (Mass PO Cancel)

**Module Info:** Cancel multiple purchase orders at once

#### Installation Test
- [ ] Module installs without errors
- [ ] Action accessible

#### Functional Tests

**13.1 Mass Cancel Access**
- [ ] Navigate to: Purchase → Orders
- [ ] Select multiple POs
- [ ] Action → Cancel Orders available

**13.2 Mass Cancel Execution**
- [ ] Select 3+ purchase orders
- [ ] Execute cancel action
- [ ] Wizard appears for confirmation
- [ ] All selected POs cancelled
- [ ] Related pickings cancelled (if applicable)

**13.3 Validation**
- [ ] Cannot cancel received POs
- [ ] Proper error message for invalid selections
- [ ] Partial success handling (some cancel, some fail)

---

### 14. stock_no_negative (Prevent Negative Stock)

**Module Info:** Prevents stock quantities from going negative

#### Installation Test
- [ ] Module installs without errors
- [ ] Location fields visible

#### Functional Tests

**14.1 Location Configuration**
- [ ] Navigate to: Inventory → Configuration → Locations
- [ ] "Allow Negative Stock" field visible on internal/transit locations
- [ ] Field hidden on other location types
- [ ] Default value is False (no negative allowed)

**14.2 Product Configuration**
- [ ] Navigate to: Products → Product form
- [ ] "Allow Negative Stock" option visible
- [ ] Can override location setting per product

**14.3 Negative Stock Prevention**
- [ ] Create outgoing transfer exceeding available qty
- [ ] Validation error appears
- [ ] Transfer blocked
- [ ] Error message clear: "Not enough stock"

**14.4 Override Scenarios**
- [ ] Enable negative stock on specific location
- [ ] Transfer succeeds with negative qty
- [ ] Quant shows negative quantity

**14.5 Edge Cases**
- [ ] Multi-location transfer validation
- [ ] Reserved quantity handling
- [ ] Concurrent transfer handling

---

## Post-Testing Checklist

### Documentation
- [ ] All test results documented
- [ ] Failed tests logged with error details
- [ ] Screenshots captured for issues
- [ ] Workarounds documented

### Issue Tracking
- [ ] Critical issues identified
- [ ] Issues prioritized (blocker/major/minor)
- [ ] Assigned to team members

### Sign-Off
- [ ] All critical tests passed
- [ ] Known issues documented and acceptable
- [ ] Stakeholder approval obtained
- [ ] Production deployment date set

---

## Test Result Summary Template

| Module | Install | Basic Function | Full Function | Notes |
|--------|---------|----------------|---------------|-------|
| stock_report_cn | ⬜ | ⬜ | ⬜ | |
| wesco_serial | ⬜ | ⬜ | ⬜ | |
| ks_dashboard_ninja | ⬜ | ⬜ | ⬜ | JS issues expected |
| ticket_management | ⬜ | ⬜ | ⬜ | Enterprise only |
| app_common | ⬜ | ⬜ | ⬜ | |
| app_odoo_customize | ⬜ | ⬜ | ⬜ | |
| bytedance_odoo_connector | ⬜ | ⬜ | ⬜ | |
| deltatech_merge | ⬜ | ⬜ | ⬜ | |
| deltatech_merge_product | ⬜ | ⬜ | ⬜ | |
| eg_cancel_stock_move | ⬜ | ⬜ | ⬜ | |
| hspl_user_simulation | ⬜ | ⬜ | ⬜ | JS issues expected |
| ks_dn_advance | ⬜ | ⬜ | ⬜ | Depends on DN |
| mass_cancel_purchase_order | ⬜ | ⬜ | ⬜ | |
| stock_no_negative | ⬜ | ⬜ | ⬜ | |

**Legend:** ✅ Pass | ❌ Fail | ⚠️ Partial | ⬜ Not Tested

---

*Checklist Version: 1.0*
*Created: January 2026*
*For: Odoo 16 → 19 Migration - Bytedance WMS Project*
