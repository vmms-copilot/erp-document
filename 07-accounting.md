# 07 — Accounting

Cash/bank movements, double-entry journal, and customer/supplier debts.

## transaction/index — Journal
- Double-entry transaction list: date, code, description, debit account, credit account, amount, party, reference.
- Uses Vietnamese chart-of-accounts style account codes (e.g. cash, bank, receivable, payable, revenue, inventory, COGS).
- Filters: date range, account, type (receipt/payment), party.

## transaction/addcash — Cash entry
- Record a cash receipt or payment: type (in/out), amount, account, party (customer/supplier/employee), reason/category, reference (order/bill/import), note, date.
- Posting creates the paired debit/credit lines automatically based on type + category.

## debts/customer — Customer debts
- Receivables ledger per customer: opening balance, charges (unpaid orders/bills), payments, closing balance.
- Formula: closing = opening + new charges - payments.
- Aging buckets (current / 30 / 60 / 90+).

### Business logic
- Every cash/bank/import/sale event posts a balanced journal entry (sum debits = sum credits).
- Customer debt is driven by unpaid order/POS balances; supplier debt by unpaid imports.
- Reports (13-reports.md) read from these entries for P&L / cash-flow.

## Entities (for schema)
- Account (id, businessId, code, name, type) — chart of accounts
- JournalEntry (id, businessId, code, date, description, partyType, partyId, refType, refId, createdAt)
- JournalLine (id, entryId, accountId, debit, credit)
- CashTransaction (id, type, amount, accountId, partyType, partyId, category, refType, refId, date)
- CustomerDebt (customerId, opening, charges, payments, closing) — derived/ledger
- SupplierDebt (supplierId, opening, charges, payments, closing) — derived/ledger
