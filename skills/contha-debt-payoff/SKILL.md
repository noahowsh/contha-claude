---
name: contha-debt-payoff
description: Use when the reader wants to pay off credit cards or loans, asks which debt to pay first, about avalanche versus snowball, or how long until they are debt free. Builds a payoff order from their balances and the rates they provide.
license: MIT
---

# Debt payoff plan

An order and a monthly amount to clear debt, from the balances in the books and the rates the reader gives.

## Steps

1. Call `get_accounts` and list every credit card and loan with its balance. Call `get_upcoming_bills` for any loan payments already recurring.
2. Ask for each debt's interest rate (APR) and minimum payment. Contha does not hold these; do not guess them.
3. Ask how much in total the reader can put toward debt each month. If they do not know, call `get_cashflow` for the last three full months and show what is usually left over.
4. Lay out both orders, and recommend one:
   - **Avalanche**: highest rate first. Least interest paid.
   - **Snowball**: smallest balance first. Quicker first win, which keeps some people going.
   Recommend avalanche unless the reader says motivation is the problem.
5. For the chosen order, show month by month for the first debt: minimums on all, everything extra on the target, until it is gone; then roll its payment into the next. Show the arithmetic plainly and say it is an estimate that ignores new charges and rate changes.
6. Offer a budget line for the monthly debt payment with `set_budget`, after a yes.

If the reader also asks about rewards cards, say paying off a balance beats any reward rate.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
