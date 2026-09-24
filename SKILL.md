---
name: drawing-t-accounts
description: Use when showing how cost or money moved through general ledger accounts, explaining a balance to an accountant or auditor, or rendering GL impact as T accounts in any medium (HTML page, NetSuite Suitelet, Word, Excel, PDF, markdown).
---

# Drawing T Accounts

## Overview

A T account shows one GL account: debits on the left, credits on the right, and the balance at the foot. The reader is an accountant, so it has to look like the textbook T and read in plain words, not in system codes.

## Normal balances by NetSuite account type

A debit increases debit-normal accounts and decreases credit-normal accounts. A credit does the opposite.

| Category | NetSuite account types (SuiteQL `account.accttype`) | Normal balance | Debit | Credit |
|---|---|---|---|---|
| Asset | Bank (`Bank`), Accounts Receivable (`AcctRec`), Other Current Asset (`OthCurrAsset`), Fixed Asset (`FixedAsset`), Other Asset (`OthAsset`), Deferred Expense (`DeferExpense`), Unbilled Receivable (`UnbilledRec`) | Debit | Increases | Decreases |
| Liability | Accounts Payable (`AcctPay`), Credit Card (`CredCard`), Other Current Liability (`OthCurrLiab`), Long Term Liability (`LongTermLiab`), Deferred Revenue (`DeferRevenue`) | Credit | Decreases | Increases |
| Equity | Equity (`Equity`), including Retained Earnings | Credit | Decreases | Increases |
| Revenue | Income (`Income`), Other Income (`OthIncome`) | Credit | Decreases | Increases |
| Cost of sales | Cost of Goods Sold (`COGS`) | Debit | Increases | Decreases |
| Expense | Expense (`Expense`), Other Expense (`OthExpense`) | Debit | Increases | Decreases |
| No GL balance | Non Posting (`NonPosting`), Statistical (`Stat`) | None | Do not draw as a T | |

Contra accounts carry the opposite balance from their type: Accumulated Depreciation (a Fixed Asset account, credit normal), owner draws or distributions (Equity, debit normal), sales returns and discounts (Income, debit normal), and labor or overhead absorption accounts (usually COGS or Expense, credit normal because they offset cost moved into inventory). Judge these by account name, not type.

A balance on the wrong side for the account's normal balance is an **abnormal balance**, for example inventory or WIP below zero. Say so in words next to the T.

**Filtered Ts show movement, not balance.** When a T is limited to one document or order, its foot is that scope's net movement, not the account's real balance. Raw Materials showing a credit for one work order is normal (material left the account). Label the foot `Net for <scope>` (a WIP or clearing T keeps `Left in <account>`) and only call a filtered balance abnormal when the scope should end at zero, like a work order's WIP.

## Layout rules

1. **Title above the T:** account number, account name, and the document or scope when the T is filtered. Example: `12302 Work-In-Process (Issued), WO500`.
2. **Two sides:** shaded `Debit` header over the left three columns, shaded `Credit` over the right three. Each side has Date, Description, Amount.
3. **Rules:** a line under the column headers, a vertical line down the middle, nothing else. No grid.
4. **One entry per row.** A debit and a credit never share a row. The empty side of a row stays blank.
5. **Sort rows by date across both sides**, then by transaction and line, so the T reads top to bottom as a timeline.
6. **Amounts right-aligned**, two decimals, thousands separators, no minus signs (the side shows the direction).
7. **Foot row** with a rule above it: balance = total debits minus total credits. Put it on the side it falls (positive on Debit, negative on Credit), shown as a positive number.
8. **Foot label:** `Balance`, or `Left in <account>` for clearing and WIP style accounts. At zero write `Balance, nothing` (or `Left in WIP, nothing`) with `0.00`.
9. **Descriptions in plain words** a controller understands: "Material issued to the order", "Assembly completed", "Order closed". Put the document number first, as a link when the medium supports it and a URL is available (plain text otherwise), and do not repeat it in the text. No internal ids, record type codes or script jargon.

## Telling a cost-flow story

When explaining a process (a work order, a close, a revaluation), use this order:

0. If the reader asked whether anything is wrong, answer that first, in plain words, above the Ts.
1. The T of the account in question (for example WIP), filtered to the scope.
2. The key event as one journal entry (the one that explains the balance: usually the close, else the entry that moved the most out of the account): a table of Account, Debit, Credit, with totals that balance.
3. "Where the other side went": one T per offsetting account. Order: inventory, then absorption or clearing, then variances, then everything else.

## Example (HTML)

```html
<div class="t-title">12302 Work-In-Process (Issued), WO500</div>
<table class="t-acct">
  <thead>
    <tr><th colspan="3" class="side">Debit</th><th colspan="3" class="side">Credit</th></tr>
    <tr><th>Date</th><th>Description</th><th class="r">Amount</th>
        <th>Date</th><th>Description</th><th class="r">Amount</th></tr>
  </thead>
  <tbody>
    <tr><td>3 Sep 2026</td><td>WOI881 Material issued to the order</td><td class="r">1,200.00</td><td></td><td></td><td></td></tr>
    <tr><td>4 Sep 2026</td><td>WOC412 Labor absorbed</td><td class="r">300.00</td><td></td><td></td><td></td></tr>
    <tr><td></td><td></td><td></td><td>5 Sep 2026</td><td>WOC412 Assembly completed, 100 units</td><td class="r">1,450.00</td></tr>
    <tr class="foot"><td></td><td><b>Left in WIP</b></td><td class="r"><b>50.00</b></td><td></td><td></td><td></td></tr>
  </tbody>
</table>
<style>
  table.t-acct { border-collapse: collapse; width: 100%; font: 12px Arial, sans-serif; }
  table.t-acct th.side { background: #f0f0f0; text-align: center; }
  table.t-acct thead tr:nth-child(2) th { text-align: left; border-bottom: 1px solid #444; }
  table.t-acct td, table.t-acct th { padding: 2px 6px; }
  table.t-acct .r { text-align: right; }
  table.t-acct td:nth-child(3), table.t-acct thead tr:nth-child(2) th:nth-child(3),
  table.t-acct th.side:first-child { border-right: 1px solid #444; }
  table.t-acct tr.foot td { border-top: 1px solid #444; }
</style>
```

In Word or Excel, draw the same six-column table with the same rules: shaded side headers, bottom border on the column header row, right border on the third column, top border on the foot row.

## Common mistakes

| Mistake | Fix |
|---|---|
| Debit and credit on the same row | One entry per row, other side blank |
| Debits listed first, then credits | Sort both sides together by date |
| Negative amounts or a signed balance | Positive numbers; the side shows direction |
| Balance always on the debit side | Foot goes on the side the balance falls |
| Calling a credit balance on an asset "normal" | Check the normal-balance table; flag it as abnormal |
| Full grid borders | Only the header rule, center line and foot rule |
| Two independent stacked columns (debits top-aligned, credits top-aligned) | One table, one row per entry, so the dates line up as a timeline |
| Calling a filtered inventory T "abnormal" | A filtered T is movement; say `Net for <scope>` |
| Descriptions like "WOCOMPL line 3 acct 412" | Plain words plus the document number |
