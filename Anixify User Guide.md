# Anixify User Guide

For the team that quotes and orders cabling and networking products.  
Covers Anixify as it is in use today (version 17.0.1.81.0).

Who can do what. Every person has one role.

| Role | What it means |
| :--- | :--- |
| **Viewer** | Can look at everything they are allowed to see. Cannot change anything. |
| **Sales** | Can do the daily work on their own quotes: quotes, OEM prices, orders, goods, delivery, returns. |
| **Admin** | Can do everything Sales can, on anyone's quotes, plus approvals, closing deals, reports and Settings. |

In this guide a tag such as `(Sales)` means "Sales and Admin". `(Admin)` means Admin only. `(everyone)` means every role, including Viewer. Your role is shown under your name at the top right of the screen.

---

## 1. Welcome and signing in

### What Anixify does

Anixify helps you quote a customer, get final prices from the OEM, order from the OEM, and follow the goods until they reach the customer. It works out every price for you, so you never calculate by hand.

It is not an accounting system. It does not keep stock accounts, payroll or invoices.

### Sign in (everyone)

1. Open the Anixify address in your browser.
2. Type your **Login** (your e-mail address) and your **Password**.
3. Click **Sign in**.

If you see `"Your session has expired — please sign in again"`, sign in again. Nothing you saved is lost.

### Change your password (everyone)

1. Click the round badge with your initials at the top right.
2. Click **Change my password**.
3. Type your **Current password**, then the **New password** twice (the second box is **Confirm new password**). The new password must have at least 10 characters.
4. Click **Change password**.

> **Good to know:** changing your password signs you out of all your other browsers. The one you are using stays signed in.

### Sign out (everyone)

Click the sign-out button, to the right of your badge at the top right.

---

## 2. Finding your way

### The side menu

The menu on the left groups the screens by the work you do. Click a group name to open or close it. The small arrow at the bottom of the menu shrinks it to icons.

| Group | Screens | Who sees it |
| :--- | :--- | :--- |
| **Sales** | Quotes, Reports | Quotes: everyone. Reports: Sales, Admin |
| **OEM** | OEM Quotes, OEM Orders, Returns, OEMs | everyone |
| **Products** | Catalog, Inventory | everyone |
| **Pricing** | Pricing Rules, Sourcing | everyone |
| **Admin** | Readiness, Settings | Readiness: Admin. Settings: everyone (some tabs Admin only) |

Screens you cannot use are hidden from your menu.

### Search

The box at the top says **Search quotes, customers, products**. Type a few letters.

- Under **Quotes**, click a quote to open it, or click **All quotes matching "..."** to see them in the Quotes list.
- Under **Customers**, click a name to see that customer's quotes.
- Under **Products**, click a part number to open it in the Catalog.

### The deal progress bar

Open any quote. Under the quote header you see a bar with nine steps. This is the whole life of a deal, from the first draft to closing it.

| Step | Name |
| :--- | :--- |
| **1** | Build the quote |
| **2** | Get OEM prices |
| **3** | Finalise the quote |
| **4** | Customer decision |
| **5** | Order from the OEM |
| **6** | Track the OEM order |
| **7** | Goods arrive at our warehouse |
| **8** | Deliver to the customer |
| **9** | Close the deal |

Each step shows one of these words:

- **Done:** finished.
- **To do now:** this is the step to work on.
- **Waiting:** waiting for someone else (for example the OEM or the customer).
- **Not yet:** an earlier step must be done first.
- **Blocked:** something is missing. The step lists what.

Click a step to jump to its section lower on the page. The step you are on is open. The others show a one-line summary.

*[Screenshot: a quote with the progress bar and the What to do now card]*

> **Good to know:** if every OEM order on a deal ships direct to the customer, steps 7 and 8 do not apply and are left out.

### The "What to do now" card

Below the bar is a card called **What to do now**. It tells you the next move in one sentence and gives you one button. Under the button it says **What happens when you click:**, so nothing is a surprise.

If the next move belongs to someone else, the card says who. If you are a Viewer, the card only says where the deal is.

### How this works

Some sections have a small help box called **How this works**. Click **Hide** to close it. You can bring it back later.

### My next actions (Sales)

At the top of the Quotes list there is a panel called **My next actions**. Click it to open. It collects what needs you, in five lists:

- My drafts with OEM prices to confirm
- My quotes waiting for the customer
- My Won quotes not yet ordered
- OEM orders: late, at risk or not yet acknowledged
- Returns and OEM claims

Click any item to open that quote. A list that says `"None: ..."` means you are up to date.

*[Screenshot: the My next actions panel, open]*

---

## 3. Your first deal, start to finish

Here is a short example with invented names. You quote 10 boxes of Cat6A cable to a customer, "Sunrise Textiles Pvt Ltd", for the project "PRJ-2026-0147".

1. **Create the quote.** On Quotes, click **New Quote**. Choose the customer, type the Project ID, click **Create Quote**.
2. **Add the cable.** Click **Add Materials**, search the part number, tick it, set the quantity, click **Add to Quote**.
3. **Price it.** Type the GP % (the margin you want). Click **Update prices**. Check the Sell Price Rs and Line Total Rs.
4. **Save the draft.** Click **Save draft**. No quote number is used yet.
5. **Ask the OEM.** In step 2, click **Request prices**. Contact the OEM yourself. When the OEM replies, click **Record** on the line and type what it confirmed.
6. **Finalise.** Click **Finalise quote** at the top right and confirm. The quote now has its number, for example Q-2026-0015, and its prices are locked.
7. **Send it.** Open **Generate Quote PDF** in the Quote Analysis panel, then **Print / Save as PDF**. E-mail the PDF to the customer yourself.
8. **Customer says yes.** Click **Mark won**. Optionally type the date you promised the customer.
9. **Order from the OEM.** In step 5, tick the line, choose Ship to, click **Place OEM order with ...**. Open **Document**, print it, send it to the OEM.
10. **Track it.** When the OEM acknowledges and ships, record each news item in step 6.
11. **Receive the goods.** When the boxes reach your warehouse, click **Record goods receipt**.
12. **Deliver.** Click **Create delivery note**, print the challan, send it with the goods. When the customer signs, click **Record proof of delivery**.
13. **Close.** An Admin clicks **Close the deal**.

> **Good to know:** Anixify never sends an e-mail. You print or save each document and send it yourself. The **What to do now** card shows you each of these moves in turn.

---

## 4. Quotes

### The quote list (everyone)

Quotes lists the quotes you may see. A draft is private to the person who started it and to Admins. Finalised quotes can be seen by everyone.

- **Columns:** Quote No., End Client, Lines, Value Rs, Status, Owner, Created.
- Click a column heading to sort. Click a row to open the quote.
- Use the search box (**Search quote no., customer, project**) and the filters: all statuses, Any OEM status, all customers, all owners (Admin), and a date range.
- **Awaiting approval** shows quotes waiting for an Admin (Admin).
- **Follow up** shows finalised, undecided quotes whose valid-until date is within 7 days or already past, soonest first (Sales).

Statuses you will see: `Draft`, `Awaiting approval`, `Finalised`, `Open`, `Won`, `Lost`, `Expired`, `Superseded`.

### Create a quote (Sales)

1. On Quotes, click **New Quote**.
2. Pick the **Customer**. Type to search by name. If the customer is not listed, click **New customer** and fill in Registered name, GSTIN (15 characters), and if useful a Ship-to and a Contact. Then click **Create customer**.
3. Type the **Project ID**. Optionally add the Project Name.
4. Check the **Destination Warehouse**.
5. Click **Create Quote**.

The Customer and the Project ID are required. If a customer with the same GSTIN or a very similar name already exists, Anixify asks **This customer may already exist**. Click **Use existing** unless you are sure.

Have the customer's PO already? See section 5, "Start a quote from a PO".

### Add materials (Sales)

1. Click **Add Materials**.
2. Search by **Search part number or description**. Narrow with Brand: All, Category: All, In stock only or Allowed brands only.
3. Tick the products and set the quantity.
4. Click **Add to Quote**.

To take a line out, use the remove button on the line ("Remove this line from the quote"). A bar appears so you can undo it.

> **Good to know:** if the customer has brand rules, the picker warns you when a product breaks them.

### Prices and margin (Sales)

Every price comes from Anixify's own pricing. You supply the OEM cost; Anixify works out the rest.

1. Check the **Pricing** switch at the top of the quote: domestic (bought in India) or import.
2. On each line, fill the cost columns: **OEM List Rs** and **OEM Disc %**, or type the **Net Price Rs** directly. You may also type Lead Time and a Remark.
3. In the header, set **GP %**. This is the margin you want on the quote. It must be above 0 and below 100.
4. Click **Update prices**. The columns **Sell Price Rs** and **Line Total Rs** fill in.
5. Read **Quote Analysis** on the right: Total Landed Cost, Margin, Final Quote Value, Realised Margin.

To see how a line's price was built, click the explain button on the line (hover text: "Explain this line's price").

*[Screenshot: the material table and the Quote Analysis panel]*

> **Good to know:**
> - A line with no price blocks saving and finalising. The quote says `"Line #2 ... has no price"`. Give it a price, or use an OEM quote price (section 6).
> - A cell shows a dash until the price is worked out. A dash is never zero.

### Import quotes

Choose import in the **Pricing** switch, then pick the **Supplier Currency**. The lines then show Conv. Rate, Duty %, Freight % and more. Freight bills are added in Quote Analysis with **Add shipment**. If the exchange rate on a line is older than today, the quote says so and offers **Use today's rate**.

### GST

GST is added for you. It is worked out from the **Dispatch from** warehouse on the quote and the customer's address.

- **Same state:** two lines, CGST and SGST.
- **Different states:** one line, IGST.

To change the warehouse, use the **Dispatch from** box in the header. Set a default in Settings (section 13). The tax appears in the quote PDF, under Material Total, before Grand Total.

### Save a draft (Sales)

Click **Save draft**. Next to it you see `Saving draft...`, `Unsaved changes` or `Draft saved`. A draft takes no quote number and is private to you and Admins.

The quote also saves by itself a moment after you change a line. Save manually before you leave the page.

### Finalise (Sales)

Finalising gives the quote its number and locks its prices. You cannot undo it.

1. Click **Finalise quote** at the top right.
2. Read the dialog, then confirm with **Finalise quote**.

If something is missing (for example no customer), the button is greyed and its hover text says what to fix. If an OEM price runs out before the normal validity, the dialog shows `"Valid until will be ... because an OEM price expires then."`

After finalising, you can still edit the manufacturer, UoM, remarks and lead times. A quote with different figures is a new quote (see "Revise" below).

### Approval below the margin limit

An Admin can set a minimum margin (Settings, section 13). A Sales user cannot finalise a quote below it without approval.

**Sales:**
1. The header button reads **Request approval** instead of Finalise quote. Click it.
2. The quote is saved and locked. Its status is `Awaiting approval`. To change it, click **Withdraw request**.

**Admin:**
1. Open the quote. The panel reads **Awaiting approval** and shows the margin against the limit.
2. Click **Approve and finalise**. The quote is finalised and gets its number.
3. Or click **Reject**, type a **Reason** (required) and click **Reject with reason**.

> **Good to know:** an Admin finalising their own low-margin quote does not need a second approval.

### Quote PDF (Sales)

1. In Quote Analysis, click **Generate Quote PDF**.
2. Click **Print / Save as PDF**. In your browser's print window, choose Save as PDF.

The PDF is made for the customer. It shows the lines, Material Total, the GST lines, Grand Total and the terms. It never shows your costs, margin, discount or OEM prices.

If the quote has unsaved changes or an unpriced line, Anixify asks you to save or price it first.

### Copy to new draft (Sales)

Click **Copy to new draft** to start a new quote with the same customer and lines. Nothing is finalised and the copy is priced again.

### Revise a finalised quote (Sales)

Use this when the figures must change after finalising, and the customer has not accepted.

1. Open the finalised quote. Click **Revise**.
2. A new draft opens. Make your changes and finalise it.
3. The new quote keeps the same number with `-R1` (then `-R2`...). The old quote becomes `Superseded`.

### Discard a draft (Sales)

Click **Discard draft**, then **Discard draft** again to confirm. The draft and its lines are removed for good. Finalised quotes cannot be discarded.

### Validity and follow-up

- Each quote gets a **Valid Until** date when it is finalised: today plus the number of days set in Settings (**Quote validity (days)**).
- Use the **Follow up** filter on Quotes to see which quotes are about to expire.
- An open quote past its date shows `Expired`. You can still mark it Won or Lost.

### Won and Lost (Sales)

Find **Outcome** in step 4, Customer decision.

**Won:**
1. Click **Mark won**.
2. Optionally type **Date promised to the customer** (all lines), or click **Set per line**.
3. Click **Mark won**.

**Lost:**
1. Click **Mark lost**.
2. Choose the **Reason**: Price, Lead time, Lost to competitor, Project cancelled, No response, or Other (Other needs a note).
3. Click **Mark lost**.

An Admin can click **Reset to open** on a Won or Lost quote.

> **Good to know:** saving a customer PO on a finalised quote marks it Won by itself.

---

## 5. Customer PO

The customer's purchase order (PO) is the customer's "yes" on paper. Anixify keeps it with the quote and checks it against your lines.

### Attach the PO document (Sales)

On the quote, find **Customer PO**. Click **Attach PO** and pick the file. It is stored with the quote. Click the file name to open it.

### Upload and compare (Sales)

Use **PO check** to compare the PO lines with the quote.

1. Either type or paste the PO lines, or click **Choose a PDF...**, then **Extract from PDF**.
2. Check what was read. Fix any row marked **Check this row**. Nothing is saved until you confirm.
3. Type the customer's PO reference.
4. Click **Save PO and compare**.

Each line gets one of these results:

| Result | Meaning |
| :--- | :--- |
| **Matches** | PO and quote agree |
| **Quantity differs** | Different quantity |
| **Unit price differs** | Different unit price |
| **Not on the quote** | The PO has a part the quote does not |
| **Quoted, not on the PO** | The quote has a part the PO does not |
| **We could not tell which line** | Anixify will not guess. Check by hand. |

> **Good to know:**
> - A PO that is a photo or scan cannot be read. Type the lines, or paste them.
> - Prices on the PO never change your quote. Anixify only compares.
> - Saved POs are listed. Use **Open**, **Compare** or **Delete PO** on each.

### Start a quote from a PO (Sales)

1. Click **New Quote**.
2. At the top, under "Have the customer's PO?", click **Start from PO** and pick the PDF.
3. Anixify creates a draft, reads the PO and shows you what it found.
4. Check the lines. Click **Add these lines to the quote** (or **Not now**).

If the PO names a customer Anixify does not know, it asks you to **Choose a customer**.

> **Good to know:** only a PDF can be read for lines. Prices always come from Anixify, never from the PO.

---

## 6. OEMs

An OEM is a manufacturer or supplier you buy from.

### The OEM list (everyone)

Open **OEMs**. It lists every OEM and every spelling used for it.

- **New OEM** adds one (Sales).
- **Add spelling** adds another way the name is written, so prices and orders still match (Sales).
- **Edit** changes contact details (Sales).
- **Merge into...** joins two entries that are the same OEM (Admin).
- **Not linked to an OEM** lists names still in use that are not tied to an OEM. An Admin links each with **Add as alias of...**.

### OEM quotes (Sales)

An OEM quote is a price list the OEM sent you. Keep it here so a customer quote can use its prices.

1. Open **OEM Quotes**. Click **New OEM quote**.
2. Choose the OEM from **Pick from the OEM list...**.
3. Fill in **OEM quote number**, **Quote date** and **Valid until**.
4. Add lines (Part number, Unit price, Lead time, Est. ship date, Shipping method) with **Add line**, or use **Upload lines (CSV / Excel)**.
5. Click **Save OEM quote**.

Search the list with **Search OEM, quote number, project or part number**. Expired quotes are marked `Expired`.

> **Good to know:** a line already used on a customer quote cannot be removed, and its part number and price cannot change.

### Use an OEM quote price on a quote line (Sales)

1. On the quote line, choose **Use OEM quote price**.
2. Anixify lists matching quotes from the same OEM for exactly that part number.
3. Click **Use this price**.

To stop using it, click **Stop using the OEM quote (keep the cost as typed)**. Save the draft to keep the change.

An OEM quote price is not final. Before ordering, the OEM must still confirm (next).

### Price requests (Sales)

A price request is your note that you asked an OEM for its final price and lead time. Anixify does not send anything. You contact the OEM yourself.

1. In step 2, Get OEM prices, click **Request prices**. One request is made for each OEM.
2. Contact the OEM by e-mail or phone.
3. Click **Mark sent** on the request. It now reads `Sent: waiting for the OEM`.

You may **Cancel** a request. You are asked for a reason.

### Record the OEM's confirmation (Sales)

1. In **OEM confirmations**, find the line and click **Record**.
2. Type the price, choose the currency, and give the **Lead time** (Days, Date or Not stated).
3. Optionally type their quote or ack number.
4. Click **Record confirmation**.

Each line shows its state, such as `OEM confirmed`, `Awaiting OEM confirmation`, `OEM price differs`, `OEM currency differs`, `OEM confirmation expired` or `OEM validity ends before the quote's`.

> **Good to know:**
> - A different confirmed price is shown, never applied. To use it, revise the quote.
> - If the OEM takes it back, click **Withdraw** and give a reason.
> - You can finalise a quote before the OEM confirms. An OEM order needs the confirmations.

### Compare offers and choose who to buy from (Sales)

When several OEMs can supply a line, step 2 shows **OEM offers**, with **Compare** on each line.

1. Click **Compare**.
2. Read each offer: Resulting sell price, Lead time, Valid until. Tags such as `Lowest price to customer` and `Fastest` help.
3. Click **Use this offer** or **Choose this offer**.
4. Anixify asks you to confirm. If it is not the lowest price, it asks **Why buy from it?**.

The line then shows **Buy from:** with that OEM. Switching OEM prices the line again. Comparing alone saves nothing.

*[Screenshot: the OEM offers comparison]*

---

## 7. Ordering from the OEM

An OEM order can be placed once the customer says yes (the quote is Won). Open step 5, **Order from the OEM**.

### The ordering checks

Anixify only lets you order what is safe to order. An order line comes only from an active, valid OEM confirmation, for a quantity the confirmation still covers, at the confirmed price. If something is not right, the screen lists it under **What's missing to place this order:**.

Common reasons: the quote is not Won, no confirmation for the line, the confirmation has expired, or the quantity is more than confirmed.

> **Good to know:** you may order the confirmed lines while others still wait for the OEM.

### Place an order (Sales)

1. Tick the lines to order (**Order line N**). The panel shows Confirmed price and Left to order.
2. Choose **Ship to**:
   - **Customer site (drop-ship):** the OEM delivers direct. You record when the customer gets it.
   - **Our warehouse:** goods come to you first. You then deliver with a delivery note (two extra steps).
3. Click **Place OEM order with [OEM name]**.

Placing takes the next OEM order number, such as `OEM-2026-0006`. It cannot be undone. One order covers one OEM.

### The order document (Sales)

1. Under **Placed OEM orders**, click **Document**.
2. Check the Order to OEM page. Click **Print / save as PDF**.
3. Send it to the OEM yourself.

> **Good to know:** Anixify does not e-mail the order.

*[Screenshot: the OEM order form with the ordering checklist]*

---

## 8. Tracking the order

Open step 6, **Track the OEM order**. Record each piece of news as the OEM gives it. Each entry is permanent. A mistake is fixed with a later entry.

### Record news (Sales)

1. Under **Placed OEM orders**, find the order. The **What to do now** card button reads **Record what [OEM] told you** and opens the form.
2. Choose what happened: **OEM acknowledged**, **Shipped**, **Delivered**, or **Cancelled** (only before anything ships). Once an order is acknowledged, the first choice becomes **OEM changed the promised date**.
3. Set **Date it happened**.
4. Type the **OEM reference** (ack number, AWB or invoice number). For a cancel, type **Why it was cancelled** (required).
5. For a shipment or delivery, type the quantity for each line, or click **Use remaining quantity**.
6. Click **Record**.

### Status

The order's status follows the entries you record.

| Status | Meaning |
| :--- | :--- |
| **Placed** | Sent, no news yet |
| **Acknowledged** | OEM has accepted it |
| **Partially shipped / Shipped** | Part or all has left the OEM |
| **Partially delivered / Delivered** | Part or all has arrived |
| **Cancelled** | Cancelled before shipping |

### Late and at risk

- **At risk:** the OEM has not acknowledged within 2 working days of the order being placed. Weekends and the holidays in Settings are skipped.
- **Late:** the order is past its promised date.

Both show as small tags. Hover over a tag to read the reason.

### Customer date

If you gave the customer a date (when marking Won, or later with **Edit dates**), the quote compares it with each OEM promise. You may see `Customer date at risk`, `OEM promise after customer date` or `Customer date not stated`. Every change is recorded.

### See every open order (everyone)

Open **OEM Orders**. The page title is **Open OEM orders**. It lists orders not yet delivered or cancelled. Filter by OEM or status. Click an order to go to its quote.

---

## 9. Goods and delivery

If the OEM ships to your warehouse, you record the goods in, then send them to the customer on a delivery note. If the OEM ships direct to the customer, skip to "Direct to customer".

### Goods receipt, called GRN (Sales)

A GRN is your record that goods reached the warehouse.

1. Open step 7, **Goods arrive at our warehouse**.
2. Click **Record goods arrived at our warehouse**. The receipt form opens.
3. Set **Date received** and the **OEM delivery note / invoice no.**
4. For each line, type **Good units now**. Add damaged units, the quantity **Per OEM document** and a **Reason** (short or damaged) if needed.
5. Click **Record goods receipt**.

The receipt takes the next GRN number. Partial receipts are normal. Short or damaged units need a reason.

> **Good to know:** a GRN is permanent. If you made a mistake, click **Reverse** and give a reason. A new reversing entry is made.

### Delivery note, called challan (Sales)

A delivery note (challan) travels with the goods to the customer. It lists items and quantities. It never shows a price or the OEM.

1. Open step 8, **Deliver to the customer**.
2. Under **Dispatch now**, type the quantity to send for each line.
3. Choose **From warehouse** and set **Date dispatched**.
4. Optionally fill E-way bill no., LR no., Transporter and Vehicle no.
5. Click **Create delivery note**.
6. Click **Challan**, then **Print / save as PDF**. Send it with the goods.

The delivery note takes the next DN number. You can send in parts.

### Proof of delivery, called POD (Sales)

POD is the customer's signed copy of the challan.

1. When the customer confirms, find the delivery note. It says `Waiting for proof of delivery`.
2. Click **Record proof of delivery**.
3. Set **Date received by the customer** (the date on the signed copy). Type **Received by** (name).
4. Optionally attach the **Signed copy**.
5. Click **Record proof of delivery**.

A mistake is fixed with **Reverse proof of delivery** and a reason.

*[Screenshot: a delivery note waiting for proof of delivery]*

### Direct to customer (drop-ship)

When the order ships to the customer's site, there is no GRN and no challan.

1. In step 6, record **Shipped** and **Delivered** news for the order.
2. When the customer confirms receipt, record it with **Record that the customer received it**.
3. Click **Attach proof of delivery** to add the signed copy. It is optional.

---

## 10. Closing a deal

When everything is delivered, finish the deal in step 9, **Close the deal**.

### Customs actuals (Sales)

For import orders, record what customs and the bank actually charged. Use **Record customs actuals** in step 7 or step 9.

1. Click **Record customs actuals**.
2. Type the **Bill of Entry no.** and the **Port**, then fill what you know: customs exchange rate, INR paid to the OEM (bank advice), Freight (INR), Insurance (INR), CHA charges (INR), Other charges (INR).
3. For each line, type the values from the Bill of Entry.
4. Click **Record Bill of Entry**.

> **Good to know:**
> - Leave a box empty if you do not know the number. It stays "not recorded". It is never read as zero.
> - These numbers feed only the "Actual" margin. They never change a price.

### Close the deal (Admin)

1. Open step 9. When everything is delivered, the card says `"Everything is delivered; ready to close"`.
2. Click **Close the deal** (on the card), or **Close order** in the Close the deal section.
3. Type the reason (required, in your own words).
4. Click **Close the deal** to confirm.

If something is still missing, the button reads **Close short (stop here)**. The dialog lists what has not been received or delivered. Closing short means you will not record the rest in Anixify.

Nothing is deleted and no figure changes. An Admin can click **Reopen order** later, with a reason.

### Quoted and actual margin (Admin)

Click **Show the Actual margin**. For each order it shows the Cost basis, Margin %, Quoted value and IGST credit, then a total. A box that says `Not computed` means a number needed to work it out has not been recorded. The panel lists what is missing.

If there were returns, **After returns** shows Credits given, Refunds given, Replacement cost, Recovered from OEMs, Unrecovered loss and the Margin after returns.

---

## 11. Returns and claims

Returns appear after goods are delivered. A customer return (called an RMA) is settled with the customer. An OEM claim recovers the cost from the supplier. One never waits for the other.

Find them in the section **Returns and OEM claims** on the quote, or on the **Returns** screen. The Returns screen lists every return and claim. Filter with `Open`, `Overdue` or `All`.

### Raise a return (Sales)

1. On the quote, click **Raise a return** (the form is **Raise a return (RMA) against delivered goods**).
2. Choose **Why is it coming back?** The reasons are: Damaged in transit, Defective / dead on arrival, Wrong item supplied, Short supplied, Failed under warranty, Commercial return (no longer needed).
3. Type **What the customer reported**.
4. Under **Coming back**, set the quantity for each line.
5. Click **Record the return**.

The return takes the next RMA number. A commercial return needs an Admin to click **Approve the return**.

Click **Acknowledgement** to print a note you can send to the customer.

### Follow the return through its steps (Sales)

The card **What to do now** names the next step. In order:

1. **Record goods back** when the goods arrive. If the goods need not come back, type why under **Or: why no return is needed**.
2. **Record inspection.** For each line, choose what you found (Defect confirmed, No fault found or Customer damage) and what happens to the goods (Send to the OEM, Return to the customer or No action).
3. **Record outcome.** Agree with the customer, line by line: Repair, Replace, Credit, Refund or No action. An Admin records a credit or refund.
4. **Close the return.** An Admin may reopen it with a reason.

> **Good to know:** returned goods never go back into stock in Anixify.

### Raise an OEM claim (Sales)

1. Click **Raise an OEM claim**. One claim covers one supplier.
2. Choose what you want: Warranty replacement, Credit note or Repair. Choose the OEM order line.
3. Click **Record the claim**. It takes the next claim number.
4. Contact the OEM. Click **Record: submitted to the OEM**.
5. When the OEM answers, record **Accepted in full**, **Partly accepted** or **Rejected**.
6. Click **Record settlement** when it is settled.

Click **Claim document** for a paper to send to the OEM. To cancel, click **Withdraw the claim**.

### Replacements and credits

**A replacement from the OEM.** When the OEM sends a replacement, click **Record replacement received (GRN)**. It is a goods receipt of its own. It never counts toward the original order.

**Send it to the customer.** Click **Create Replacement delivery note**. The challan is marked `Delivery Challan (Replacement)`.

**Or order a new one.** Click **Record OEM confirmation** (type 0 for a free warranty replacement), then **Place replacement order**. The usual ordering checks apply.

**A credit or refund to the customer.** An Admin records it as an amount and a reference. It is a note of what was agreed, not an accounting entry.

### Overdue

A return or claim is `Overdue` when it is waiting too long. The days are set in Settings, Company: **Goods not back from the customer after (days)** and **No answer from the OEM after (days)**.

---

## 12. Reports and Readiness

### Reports (Sales)

Open **Reports**.

1. Choose **From** and **To** dates. The report counts quotes by their quote date.
2. Read the figures: how many quotes are Drafts, Finalised and Won, the Win rate (Won out of Won plus Lost), and the Lost reasons.
3. Switch between **By customer**, **By salesperson** and **By month**.
4. Click **Export to Excel** to save the table.

Sales sees **Your quotes only**. Admin sees **Everyone's quotes**.

> **Good to know:** a quote that cannot be priced is counted but left out of value and GP.

### Readiness (Admin)

Open **Readiness**. The page is called **What's blocking quotes**. It lists drafts that cannot be priced or finalised, with the reason. It is read only. A number on the menu item shows how many are blocked. `Nothing is blocking any draft.` means all is well.

---

## 13. Settings

Open **Settings**. Sales and Viewer see some tabs. Admin sees all. Tabs: `Exchange Rates`, `Duty Rates`, `Warehouses`, `Holidays`, `Company` and `Users`.

### Company (Admin)

1. Open the **Company** tab.
2. Fill the details you need and click **Save**.

| Setting | What it does |
| :--- | :--- |
| **Registered Address, GSTIN, company details** | Printed on your documents |
| **Default dispatch warehouse** | Used for new drafts. Drives GST |
| **Quote validity (days)** | Sets Valid Until when a quote is finalised |
| **Approval required below GP %** | The margin limit for approvals (section 4) |
| **Quote numbering** | Prefix and Digits of quote numbers |
| **OEM order numbering** | Same, for OEM orders |
| **Numbering for GRN, delivery note, return (RMA) and OEM claim** | Same, one block each |
| **Returns and OEM claims: when is one overdue** | Days before a return or claim shows Overdue |
| **Default GST rate (%)** | Tax rate used on new quotes |
| **Number format (locale), Currency symbol, Amount in words** | How money is written. Lakh / Crore or Thousand / Million |
| **Our entity names on customer POs** | Helps Anixify read your customers' POs |
| **Company Logo** | Upload a PNG, JPEG, GIF or WEBP image |

> **Good to know:** a numbering change applies from the next document. Numbers already issued never change. Validity applies to quotes finalised from now on.

### Users (Admin)

1. Open the **Users** tab.
2. Click **Add user**. Type the Name, Login (e-mail), Role and an Initial password (at least 10 characters). Click **Add user**.
3. To change a person, click **Edit**, change name, login or role, then **Save**.
4. To set a new password, click **Reset password**, type the New password, then **Set**. Their open sessions are signed out.
5. To stop someone using Anixify, deactivate them. Users are never deleted. The list shows **Last sign-in**.

### Warehouses (Admin to change, everyone to see)

The warehouses a quote can be sent from. A quote's GST uses its dispatch warehouse.

- Edit a warehouse to change its address, GSTIN or contacts.
- **Upload CSV** adds or updates many at once. Anixify shows a preview. Click **Confirm import**. If any row has an error, nothing is imported.
- Nothing is deleted. Deactivate a warehouse instead.
- A warehouse marked `FTWZ` cannot be chosen on a quote.

### Holidays (Admin to change)

The company's non-working days. They are used only to decide when an OEM order is `At risk`.

1. Type the date and a Holiday name, click **Add holiday**.
2. Or click **Upload CSV / Excel** with columns `Date (YYYY-MM-DD)` and `Name`, and click **Apply**.
3. Use **Rename** to correct a name, or deactivate a holiday.

With no holidays listed, working days are Monday to Friday.

### Exchange Rates (Admin to publish, everyone to see)

Used for import quotes.

1. Click **Publish rate**.
2. Choose the Currency, type the rate in INR per unit, and set the date it starts.
3. Confirm. If the rate differs a lot from the current one, Anixify asks you to confirm.

A rate cannot start in the past. The previous rate ends the day before. To fix a mistyped rate, click **Correct** on it. A correction is refused if a finalised quote already used that rate.

### Duty Rates (Admin)

Customs duty by HSN code (an 8-digit product code).

1. Click **Publish rate**.
2. Type the HSN (8 digits), BCD %, SWS % and IGST %, and the start date.
3. Click **Publish rate**, then **Yes, publish**.

Or use the upload box (CSV or Excel), check the preview and **Confirm and save**. A rate cannot start in the past. Use **Correct** for a typing mistake. Filter by `In force` or `Starts later`.

### Discount rules (Admin)

Open **Pricing Rules**, then the **Discounts** tab. A rule is a standing discount a customer has agreed with an OEM.

1. Click **Add Discount**.
2. Choose the Customer and the OEM. Type the OEM Discount % and the Agreed On date. Add Notes if useful.
3. Click **Create rule**.

There is one rule for each customer and OEM pair. To replace it, **Archive** the old one first. **Reactivate** brings it back. Sales sees the rules but cannot change them.

> **Good to know:** a competitive bid is not a rule. Type the OEM's net cost on the quote line instead.

### Catalog and uploads (Sales)

Open **Catalog** to see the products.

- Search by **Search part number or description**. Filter by Brand: All, Category: All, Origin: All. Click **Clear** to reset.
- **Add Product** adds one product. Choose a brand and a category (both needed). Click **Create Product**. A part number that already exists is allowed with a warning.
- **Import CSV** adds up to 200 products from a file. Anixify checks the file first, then imports.

The Catalog holds no prices. Prices are worked out on each quote line.

---

## 14. Glossary

| Word | Meaning |
| :--- | :--- |
| **OEM** | The manufacturer or supplier you buy from. |
| **OEM quote** | A price list an OEM sent you. Not final until the OEM confirms. |
| **Confirmation** | The OEM's final price and lead time for a line, as you record it. |
| **Drop-ship** | The OEM delivers direct to the customer's site. |
| **Finalise** | Give the quote its number and lock its prices. |
| **Draft** | A quote that is not finalised. It has no number. |
| **Superseded** | A quote replaced by a revision. |
| **PO** | The customer's purchase order. |
| **GP / margin** | The part of the sell price that is your profit, as a percentage. |
| **GRN** | Goods receipt note. Your record that goods reached your warehouse. |
| **DN / challan** | Delivery note. The paper that travels with goods to the customer. It shows no price. |
| **POD** | Proof of delivery. The customer's signed copy of the challan. |
| **RMA** | A customer return. Short for return merchandise authorisation. |
| **Claim** | A request to the OEM to put right faulty or damaged goods. |
| **HSN** | An 8-digit product code used for customs duty and GST. |
| **GSTIN** | A company's 15-character GST number. |
| **Ship-to** | The address the goods go to, which may differ from the billing address. |
| **BoE** | Bill of Entry. The customs paper for an import. |
| **BCD / SWS / IGST** | The three parts of Indian import duty. BCD is basic customs duty. SWS is a surcharge on the BCD. IGST is the tax on the total. |
| **FTWZ** | Free trade warehousing zone. Such warehouses cannot be chosen on a quote. |
| **At risk** | An OEM order with no acknowledgement within 2 working days. |
| **Late** | An OEM order past its promised date. |

---

## 15. Appendix

### Not in use yet

These screens exist but are not ready. Do not rely on them.

- **Inventory:** shows no live stock yet.
- **Risk Buffers (in Settings):** view only.
- **Brand Locks (in Pricing Rules):** not set up.
- **Freight Rates (in Sourcing):** read only. The Sourcing screen is a look-up. Publish duty rates in Settings.

### Getting help

- Start with the **What to do now** card and the **How this works** boxes on the quote.
- If a button is missing, check your role under your name at the top right. A Viewer cannot change anything.
- If something does not work, tell your Admin. Include the quote number and what you clicked.