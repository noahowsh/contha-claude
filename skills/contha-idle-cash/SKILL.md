---
name: contha-idle-cash
description: Use when the reader asks whether their savings are earning enough, where to keep cash, about high-yield savings accounts, or how much is sitting idle in checking. Finds cash earning less than it could and how much to leave where it is.
license: MIT
---

# Idle cash

Finds cash earning less than it could, and how much of it can actually move without leaving the bills short.

## Steps

1. Call `get_savings_opportunities`. Each account over $1,000 shows its balance, what it earns now (worked out from the interest it has paid), the best rate on record and where, and what a year of the difference comes to.
2. Call `get_overview` for Free to use, and `get_goals` for which accounts fund goals (`fundedFrom`).
3. How much can move, account by account:
   - Checking: at most Free to use, less a buffer the reader chooses (suggest about a week of spending from `get_cashflow`). Free to use has already taken off the bills, cards and goal money, so do not take them off again.
   - Savings: the whole balance can move to a better rate; savings keeps no bills.
   - An account that funds a goal can move too, but the goal follows the account: say the reader should point the goal at the new account in the app after moving. Do not call goal money idle; it is idle only in the sense of earning little.
   Show what moves, subtracting step by step.
4. Present a table: account, balance, earning now, best rate and where, a year of the difference, and how much you suggest moving.
5. Say: rates are as last checked on the date shown, not live; the reader opens the account and moves the money themselves; savings at a federally insured bank or credit union are insured up to the limit, and they should check that the account they pick is.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
