---
name: contha-month-in-review
description: Use at the end or start of a month, or when the reader asks how last month went, for a monthly money recap, savings rate, or a month-over-month comparison. Writes a short monthly report from their books.
license: MIT
---

# Month in review

A one-screen recap of the month just finished, compared with the one before.

## Steps

1. Pick the month: the last full month unless the reader names one. Call `get_cashflow` with `from` and `to` covering that month and the month before. Read `months` (each with readable income, spend and net), `incomeBySource` and `spendByGroup`.
2. Call `get_spending_trends` for the last six months to see which categories ran above or below their usual.
3. Call `get_budget` for the month if the reader keeps one.
4. On the Business plan, call `get_profit_and_loss` for the year and take that month's business column.
5. Write:
   - **The month**: income, spending, and what was left, as the tools give them. Savings rate: what was left divided by income, with the division shown.
   - **Against last month**: up or down, in dollars, for income, spending and left over.
   - **What moved**: the two or three categories furthest from their usual, with figures.
   - **Budget**: lines over, lines with room (if a budget exists).
   - **Business** (Business plan): the business's profit for the month.
6. End with one thing to keep and one thing to change next month.

Keep it under 250 words. No grades, no streaks.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
