# Family Wealth Tracker — Requirements

Status: requirements only, no code yet. Last updated 2026-11-06.

## 1. Platform
- iOS app built with SwiftUI
- All amounts in US dollars (USD)

## 2. Security and Unlock
- Users securely unlock the app before seeing any financial data
- Initial local-first version: unlock with Face ID, or the device passcode as a fallback (no online account)
- Future: add user authentication (for example Sign in with Apple) when multi-device sync and public accounts are introduced

## 3. Family Groups (first screen after unlock)
- The first screen after unlock lists the user's Family Groups
- Each group row shows only the group name and its total net worth
- Example groups: Couple, Parents + Evelyn, Entire Family
- A person can belong to multiple groups
- Users can create, rename, and delete groups, and add or remove members

## 4. Family Group Dashboard
- Tapping a group opens its financial dashboard
- The dashboard shows four sections:
  - Liquid Assets
  - Retirement Assets
  - Non-Liquid Assets
  - Liabilities
- Total Net Worth is shown at the bottom (Liquid + Retirement + Non-Liquid − Liabilities)
- Each section is tappable and opens its categories
- The dashboard also shows an Income & Expenses section (see Section 9). It is separate from net worth and not included in the Total Net Worth formula

## 5. Category Drill-Down
- Tapping a section shows its categories, for example:
  - Liquid Assets: Cash, Stocks, other investments
  - Retirement Assets: 401(k), traditional IRA, Roth IRA, and similar accounts
  - Non-Liquid Assets: property, vehicles, valuables
  - Liabilities: mortgages, loans, credit cards
- Tapping a category lists its individual accounts
- Each account row shows its name, value, and owner(s)

## 6. Retirement Assets
- Retirement Assets is its own section, separate from Liquid Assets, because of withdrawal restrictions
- Includes 401(k), traditional IRA, Roth IRA, and similar accounts
- Retirement assets count toward total net worth

## 7. Individual Account Details
- Current balance
- Historical value chart
- Account owner(s) and their ownership percentages
- Latest available update time (when the balance was last refreshed or edited)

## 8. Ownership and Counting Rules
- Each account or asset can have one or more owners
- Each account is stored only once, even when its owners belong to multiple groups, so it is never double-counted
- Default ownership for two owners is 50% each
- Users can edit ownership percentages (for example 70/30)
- Ownership percentages must total exactly 100%
- A group's net worth includes only the ownership shares that belong to that group's members
  - Example: a $100,000 account owned 70/30 by A and B counts $70,000 in a group with only A
- If all owners of an account are in the same group, the full value is counted once
- An account appears in every group that includes at least one of its owners

## 9. Income and Expenses
- Track money coming in (income) and going out (expenses)
- Each entry has an amount, date, category, and the person(s) it belongs to

### 9.1 Income & Expenses Dashboard
- Each Family Group dashboard has an Income & Expenses section
- Tapping it opens a screen showing monthly income, expenses, and net cash flow
- Recurring and variable transactions are shown separately
- Family Group membership and ownership-percentage rules (Section 8) apply to income and expenses, the same way they apply to accounts

### 9.2 Recurring Income and Expenses
- Users create recurring entries manually
- Each entry has an amount, category, description, frequency, and expected payment date
- Examples: mortgage, daycare, tuition, subscriptions, insurance, salary
- A recurring entry's amount is either:
  - **Fixed:** the same every time (e.g. a subscription)
  - **Estimated:** expected to vary month to month (e.g. a utility bill)
- Recurring entries can be edited, paused, or deleted

### 9.3 Variable Income and Expenses
- Supports transactions imported from connected financial accounts and transactions entered manually
- Transactions are categorized automatically when possible, and users can correct the category manually
- Monthly totals are shown by category
- Credit card purchases count as expenses
- Payments toward a credit card balance are not counted as additional expenses (the purchases were already counted)

### 9.4 Duplicate Detection
Runs whenever the user manually refreshes connected financial accounts:
- Imported transactions are compared against the user's recurring income and expense entries
- A possible match is flagged when the transaction amount exactly matches a recurring entry
- Transaction date and merchant information are also considered when available
- A warning shows that the transaction may be a duplicate
- Transactions are never automatically merged or deleted
- Linking two matching entries requires user confirmation
- **Confirmed:** the transaction is counted only once in financial summaries
- **Rejected:** both transactions stay separate and are both counted

### 9.5 Needs Review Screen
- One central Needs Review screen lists all possible duplicate matches
- Each item shows the recurring entry side by side with the imported transaction
- Users can confirm or reject matches one at a time or in bulk
- Confirmed and rejected decisions are saved and not asked again
- Unresolved matches stay visible until reviewed

### 9.6 Monthly Summary
Shows, for the selected Family Group and month:
- Total monthly income
- Recurring expenses
- Variable expenses
- Total expenses
- Net monthly cash flow (total income − total expenses)
- Number of transactions awaiting review

Recurring entries and imported transactions are never double-counted.

## 10. Account Connections and Manual Entry
- Link bank and brokerage accounts through a connection provider
- Balances and transactions update only when the user taps refresh (no automatic background refresh)
- Manually add and edit other assets (e.g. property, cash, valuables) and debts (e.g. loans, credit cards)
- Manual and connected accounts appear the same way in groups and drill-downs

## 11. Monthly Snapshots and History
- Save a value snapshot once a month for each account, group, and net-worth total
- Snapshots power the trend and account history charts
- Each refresh or manual edit also records a dated value for the account history

## 12. Data Storage and Sync
- Data is stored locally on the device first
- iCloud sync is a future feature
- Family sharing (other family members using their own devices) is a future feature
- The data model is designed so sync and sharing can be added later without starting over

## 13. Multiple Households (future public release)
- The data model supports multiple independent households
- Every person, account, group, transaction, and snapshot belongs to exactly one household
- Households never see each other's data

## 14. Privacy
- Strong privacy by default: financial data stays on the device unless the user turns on sync
- No ads, no tracking, and no sending data to third parties beyond the account-connection provider
- Bank login details are never stored by the app

## 15. Changes to Earlier Requirements
- "Opens to a combined family dashboard" is replaced by the Family Groups list (Section 3). The "Entire Family" group gives the combined view
- "Drill down per person" is replaced by group → section → category → account drill-down. Each account still shows its owner(s)
- "Sign in" for the first version means local unlock only (Section 2)
- Retirement accounts moved out of Liquid Assets into their own Retirement Assets section (Section 6)

## 16. Resolved Questions
- Sign-in vs. local-first: Face ID / passcode now, user authentication later
- Retirement liquidity: separate Retirement Assets section
- Joint accounts across groups: count only the shares owned by group members
- Income and expenses on the dashboard: an Income & Expenses section on each group dashboard, using the same membership and ownership rules (Section 9)

## 17. Open Questions
- **Q1. Three or more owners:** default to an equal split (e.g. 33.33% each)? How should rounding be handled so the shares total exactly 100%?
- **Q2. Shared liabilities:** do the same ownership-share rules apply to debts like a joint mortgage? (Assumed yes)
- **Q3. Per-person view:** should there also be a view of a single person's accounts, or is the group drill-down enough?
- **Q4. History before the app existed:** connected accounts may provide past balances, but manual assets only start their history when first entered. Is that acceptable?
- **Q5. Ownership changes over time:** if a split changes (e.g. 50/50 to 70/30), should past snapshots keep the old split?
- **Q6. Estimated amounts vs. exact matching:** duplicate detection flags exact amount matches, but estimated recurring entries (Section 9.2) will rarely match exactly. Should estimated entries match within a tolerance (e.g. ±10%), or rely on date and merchant instead?
- **Q7. Recurring entries with no imported match:** if a recurring expense is expected but no matching transaction has been imported yet, does the monthly summary count the expected amount, the actual amount only, or show both "expected" and "actual"?
- **Q8. Owner of a transaction:** imported transactions can inherit the owners of their account. Who owns a manual transaction or recurring entry by default, and can it be split (e.g. 50/50 daycare)?
- **Q9. Transfers between own accounts:** like credit card payments, moving money between the family's own accounts (e.g. checking to savings, or a loan payment) shouldn't count as income or expense. Should these be detected and excluded the same way?
- **Q10. Refunds and returns:** should a refund reduce expenses in its category, or count as income?
- **Q11. Match date window:** how many days before or after the expected payment date should a transaction still be treated as a possible match?
- **Q12. Pausing vs. deleting:** when a recurring entry is deleted, should its past confirmed matches and history be kept?
