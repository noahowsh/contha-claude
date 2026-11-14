---
name: contha-emergency-fund
description: Use when the reader asks if they have enough of an emergency fund, how many months of expenses they could cover, how much to keep in savings, or how to build a safety net. Measures their cushion in months from their own spending.
license: MIT
---

# Emergency fund check

How many months the reader could cover from savings if income stopped, and what it would take to reach their target.

## Steps

1. Call `get_cashflow` with `from` the first day and `to` the last day of the last six full months (not the default range, which starts and ends mid-month). Take monthly spending from `months`.
2. Call `get_goals`. The emergency fund is the goal for it: its `saved` is the fund, up to its target (past the target, `overTarget` says the rest is not held back, so it is spendable, not fund). Free to use already holds that money back (`heldInFreeToUse`), so it is not also spendable.
3. If there is no such goal, call `get_accounts` and ask which savings account is the fund. Do not count an account that funds another goal (`fundedFrom` in `get_goals`), and do not count checking beyond Free to use from `get_overview`: the rest of checking is already spoken for by bills and cards. Keep the business's cash out of it on the Business plan.
4. Work out, showing each step:
   - Essential monthly spending: the average of the months, or, if the reader prefers, the essentials only (rent, bills and groceries from `get_spending_trends`).
   - Months covered: the fund divided by that monthly figure.
   - The gap to the target: the goal's own `remaining` if there is a goal; otherwise three to six months is the usual range (self-employed readers often aim higher because income is lumpy).
5. If there is a goal, quote its `neededEachMonth` and whether it is `behind`. If there is none, offer to suggest one; the reader creates it on the Goals page.
6. If the fund is earning little, offer the idle cash skill.

Say clearly that this is the reader's own arithmetic on Contha's figures, and that the right size depends on their job, health and dependents.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
