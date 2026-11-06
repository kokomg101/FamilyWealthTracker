# Family Wealth Tracker — Open Questions

The central list of unresolved product and technical design questions.

## How this file works
- Each question has a permanent ID (`OQ-###`). IDs are never reused or renumbered.
- **Status** is one of:
  - **Open:** not yet discussed
  - **Needs Discussion:** discussed, but no decision yet
  - **Resolved:** decision confirmed by the product owner
- A recommendation is only a suggestion. A question becomes Resolved only after the product owner confirms the decision.
- Once a question is resolved, its decision is added to [requirements.md](requirements.md), which lists only agreed functionality.
- New questions are added at the end of the Open section with the next free ID.
- OQ-001 to OQ-012 were previously Q1 to Q12 in Section 17 of `requirements.md`.

## Summary

| ID | Category | Question | Status |
|---|---|---|---|
| OQ-001 | Ownership | Default split for three or more owners | Open |
| OQ-002 | Ownership | Ownership shares for shared liabilities | Open |
| OQ-003 | Navigation | Per-person view | Open |
| OQ-004 | History | History before the app existed | Open |
| OQ-005 | Ownership / History | Ownership changes over time | Open |
| OQ-006 | Duplicate Detection | Estimated amounts vs. exact matching | Open |
| OQ-007 | Income & Expenses | Recurring entries with no imported match | Open |
| OQ-008 | Ownership / Income & Expenses | Owner of a manual transaction or recurring entry | Open |
| OQ-009 | Income & Expenses | Transfers between the family's own accounts | Open |
| OQ-010 | Income & Expenses | Refunds and returns | Open |
| OQ-011 | Duplicate Detection | Match date window | Open |
| OQ-012 | Income & Expenses | Deleting recurring entries | Open |
| OQ-013 | Security | Sign-in vs. local-first | Resolved |
| OQ-014 | Assets | Retirement account liquidity | Resolved |
| OQ-015 | Ownership | Joint accounts across groups | Resolved |
| OQ-016 | Dashboard | Where income and expenses appear | Resolved |

---

## Open Questions

### OQ-001 — Default split for three or more owners
- **Category:** Ownership
- **Question:** For accounts with three or more owners, should ownership default to an equal split (e.g. 33.33% each)? How should rounding be handled so the shares total exactly 100%?
- **Background / why it matters:** Two owners default to 50/50 (requirements Section 8), but three-way splits don't divide evenly. Shares must total exactly 100%, or group totals will be slightly off.
- **Options:**
  1. Equal split, with the rounding leftover given to one owner (e.g. 33.34 / 33.33 / 33.33)
  2. No default: the user must enter every percentage
  3. Equal split, stored internally as fractions (1/3) instead of rounded percentages
- **Recommendation:** Option 1. It's a sensible default that always adds up to exactly 100%, and the user can still edit the split.
- **Status:** Open
- **Final decision:** —

### OQ-002 — Ownership shares for shared liabilities
- **Category:** Ownership
- **Question:** Do the same ownership-share rules apply to debts, such as a joint mortgage?
- **Background / why it matters:** The current assumption is yes. If debts were handled differently, a group's net worth could include a shared house at 50% but its mortgage at 100%, which would be misleading.
- **Options:**
  1. Same rules as assets: one or more owners, editable percentages, group counts only its members' shares
  2. Always count the full debt for any group containing an owner
- **Recommendation:** Option 1. It keeps assets and debts consistent, and net worth stays accurate for every group.
- **Status:** Open
- **Final decision:** —

### OQ-003 — Per-person view
- **Category:** Navigation
- **Question:** Should there be a view of a single person's accounts, or is the group drill-down enough?
- **Background / why it matters:** The original requirement included drilling down per person. It was replaced by group → section → category → account drill-down, but seeing one person's finances may still be useful.
- **Options:**
  1. No per-person view: the group drill-down is enough
  2. Allow one-person Family Groups (e.g. "Evelyn"), reusing the same dashboard
  3. Build a separate per-person screen
- **Recommendation:** Option 2. It gives a per-person view with no extra screens, and the ownership rules already work for a group of one.
- **Status:** Open
- **Final decision:** —

### OQ-004 — History before the app existed
- **Category:** History
- **Question:** Connected accounts may provide past balances, but manual assets only start their history when first entered. Is that acceptable?
- **Background / why it matters:** Net-worth trend charts will look incomplete before the app's start date, especially for manually entered assets like property.
- **Options:**
  1. Accept it: history starts when each asset is added
  2. Let users optionally enter past values (e.g. a home's value last year)
  3. Import past balances from connected accounts where available, and accept gaps for manual assets
- **Recommendation:** Options 1 and 3 for the first version, with Option 2 as a later improvement. This keeps the first version simple without blocking backfill later.
- **Status:** Open
- **Final decision:** —

### OQ-005 — Ownership changes over time
- **Category:** Ownership / History
- **Question:** If an ownership split changes (e.g. 50/50 to 70/30), should past snapshots keep the old split?
- **Background / why it matters:** Recalculating history with the new split would silently change past net-worth numbers for each group.
- **Options:**
  1. Past snapshots keep the split that applied at the time
  2. All history is recalculated with the current split
- **Recommendation:** Option 1. Snapshots act as a fixed record, so history doesn't change unexpectedly.
- **Status:** Open
- **Final decision:** —

### OQ-006 — Estimated amounts vs. exact matching
- **Category:** Duplicate Detection
- **Question:** Duplicate detection flags exact amount matches, but estimated recurring entries (requirements Section 9.2) will rarely match exactly. Should estimated entries match within a tolerance (e.g. ±10%), or rely on date and merchant instead?
- **Background / why it matters:** Without a rule for estimated entries, bills like utilities would never be flagged, and they would be double-counted.
- **Options:**
  1. Exact match only (estimated entries are never flagged)
  2. Fixed entries need an exact match; estimated entries match within a tolerance (default ±10%, user-adjustable)
  3. Estimated entries match on date and merchant only, ignoring the amount
- **Recommendation:** Option 2. Fixed entries stay strict and estimated bills still get caught. Combining the tolerance with the date window (OQ-011) keeps false matches low, and the user always confirms.
- **Status:** Open
- **Final decision:** —

### OQ-007 — Recurring entries with no imported match
- **Category:** Income & Expenses
- **Question:** If a recurring expense is expected but no matching transaction has been imported yet, does the monthly summary count the expected amount, the actual amount only, or show both "expected" and "actual"?
- **Background / why it matters:** Some recurring payments come from accounts that aren't connected (e.g. daycare paid in cash), so they never get imported. If only actual amounts count, those expenses disappear. If expected amounts count, a matching import may double-count it.
- **Options:**
  1. Count actual imported amounts only
  2. Count the expected amount once its due date passes; when a match is confirmed, replace it with the actual amount
  3. Show both expected and actual columns, with totals based on actual
- **Recommendation:** Option 2. Manual-only bills are still counted, and confirmed matches prevent double-counting. Unconfirmed possible matches already appear in "Transactions awaiting review".
- **Status:** Open
- **Final decision:** —

### OQ-008 — Owner of a manual transaction or recurring entry
- **Category:** Ownership / Income & Expenses
- **Question:** Imported transactions can inherit the owners of their account. Who owns a manual transaction or recurring entry by default, and can it be split (e.g. 50/50 daycare)?
- **Background / why it matters:** Group income and expense totals depend on ownership shares (requirements Section 9.1), so every entry needs owners.
- **Options:**
  1. The user must choose owners for each entry, with no default
  2. Optionally link the entry to the account it's paid from and inherit that account's owners; otherwise the user picks owners. Multiple owners default to an equal split and are editable
  3. Always owned by the person who created it
- **Recommendation:** Option 2. It reuses the existing ownership rules, keeps entry quick, and supports splits like 50/50 daycare.
- **Status:** Open
- **Final decision:** —

### OQ-009 — Transfers between the family's own accounts
- **Category:** Income & Expenses
- **Question:** Like credit card payments, moving money between the family's own accounts (e.g. checking to savings, or a loan payment) shouldn't count as income or expense. Should these be detected and excluded the same way?
- **Background / why it matters:** Without this, moving $1,000 to savings appears as $1,000 of spending, and cash flow looks wrong. Loan and mortgage payments are less clear, because the mortgage is already listed as a recurring expense.
- **Options:**
  1. Ignore transfers (they count as income and expenses)
  2. Detect likely transfers (matching opposite amounts between the family's accounts within a few days) and flag them for confirmation, like duplicates. Loan and mortgage payments still count as expenses
  3. Same as Option 2, but also exclude loan payments, treating them only as a reduction in the debt
- **Recommendation:** Option 2. It matches how people think about monthly bills, since mortgage payments feel like an expense, and it follows the existing "never decide automatically" rule.
- **Status:** Open
- **Final decision:** —

### OQ-010 — Refunds and returns
- **Category:** Income & Expenses
- **Question:** Should a refund reduce expenses in its category, or count as income?
- **Background / why it matters:** Counting refunds as income would overstate both income and spending.
- **Options:**
  1. Reduce expenses in the original category
  2. Count as income
  3. Ask the user each time
- **Recommendation:** Option 1. A returned purchase was never really spent, so category totals stay accurate. Users can still re-categorize individual refunds.
- **Status:** Open
- **Final decision:** —

### OQ-011 — Match date window
- **Category:** Duplicate Detection
- **Question:** How many days before or after the expected payment date should a transaction still be treated as a possible match?
- **Background / why it matters:** Payments often post a few days early or late (weekends, holidays, bank processing). A window that's too narrow misses duplicates, and one that's too wide creates false warnings.
- **Options:**
  1. ±3 days
  2. ±5 days default, adjustable per recurring entry
  3. Anywhere in the same calendar month
- **Recommendation:** Option 2. It covers normal posting delays and weekends, and adjusting it per entry handles irregular payers.
- **Status:** Open
- **Final decision:** —

### OQ-012 — Deleting recurring entries
- **Category:** Income & Expenses
- **Question:** When a recurring entry is deleted, should its past confirmed matches and history be kept?
- **Background / why it matters:** Deleting history would change past monthly summaries and could bring back duplicates the user had already resolved.
- **Options:**
  1. Delete everything, including past history
  2. "Delete" stops future occurrences but keeps past history and matches (soft delete), with a separate option to remove the history permanently
  3. Only allow pausing, not deleting
- **Recommendation:** Option 2. Past summaries stay accurate, and users can still remove entries they no longer need.
- **Status:** Open
- **Final decision:** —

---

## Resolved Questions

These were decided earlier and moved here from Section 16 of `requirements.md`. Their decisions are already in the requirements.

### OQ-013 — Sign-in vs. local-first
- **Category:** Security
- **Question:** "Sign in" normally needs an online account, but the first version stores data locally with no server. What does sign-in mean?
- **Background / why it matters:** It determines whether the first version needs an account system.
- **Options:** (1) Local unlock only; (2) Online account from the start
- **Recommendation:** Option 1, adding authentication later.
- **Status:** Resolved
- **Final decision:** Face ID or device passcode for the local-first version. User authentication will be added when multi-device sync and public accounts are introduced. (requirements Section 2)

### OQ-014 — Retirement account liquidity
- **Category:** Assets
- **Question:** Should 401(k) and IRA accounts count as Liquid Assets, given withdrawal restrictions?
- **Background / why it matters:** Treating them as liquid overstates money that's actually available.
- **Options:** (1) Non-Liquid by default; (2) Liquid but marked restricted; (3) User chooses per account; (4) Separate section
- **Recommendation:** —
- **Status:** Resolved
- **Final decision:** A separate Retirement Assets section that counts toward net worth but is kept apart from Liquid Assets. (requirements Sections 4–6)

### OQ-015 — Joint accounts across groups
- **Category:** Ownership
- **Question:** If a joint account's owners are in different groups, does a group count the full balance or only its members' share?
- **Background / why it matters:** It decides whether group totals double-count shared accounts.
- **Options:** (1) Full balance; (2) Members' shares only
- **Recommendation:** Option 2.
- **Status:** Resolved
- **Final decision:** Count only the shares owned by group members, with editable ownership percentages that default to 50/50 for two owners. Full value is counted once when all owners are in the group. (requirements Section 8)

### OQ-016 — Where income and expenses appear
- **Category:** Dashboard
- **Question:** Where do income and expenses appear, and do they follow group and ownership rules?
- **Background / why it matters:** The group dashboard originally showed only assets and liabilities.
- **Options:** (1) Separate tab; (2) Section on each group dashboard
- **Recommendation:** —
- **Status:** Resolved
- **Final decision:** An Income & Expenses section on each group dashboard, using the same membership and ownership rules. (requirements Section 9)
