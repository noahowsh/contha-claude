---
name: contha-paycheck-plan
description: Use when the reader just got paid or is about to, and asks what to do with the paycheck, how to split it, how much to save, or what has to be covered before the next one. Plans the paycheck against bills, budget and goals.
license: MIT
---

# Paycheck plan

Assigns a paycheck before it gets spent: bills before the next one, then goals, then what is free.

## Steps

1. Get the paycheck amount and the date of the next one. A household may have more than one earner on different schedules; ask which paycheck. If the reader does not know the date, call `get_transactions` for the last 45 days and read the deposits filed as the paycheck category by their `merchant` (the employer) and dates; `incomeBySource` in `get_cashflow` groups by category, not by employer.
2. Call `get_overview`. Free to use already holds back every bill in the next 30 days, what is owed on cards, goal money and, on the Business plan, tax. So the paycheck is new money on top of it: nothing it holds back is taken from the paycheck again.
3. Call `get_goals` for each goal's `neededEachMonth` (skip a goal marked `taxReserve`: its money is the tax, already held back), and `get_budget` for flexible lines with room left.
4. Lay out the plan as a short table, each figure from the tools:
   - Goal contributions for this pay period: a month's need, halved for a twice-a-month paycheck (show the division).
   - Flexible spending until the next paycheck, from the budget's remaining room.
   - What is left of the paycheck: free to spend or to put toward a goal early.
   Then one line: Free to use today plus what is left of the paycheck is what the reader can spend before the next one, with the sum shown.
5. If goals do not fit, say which goal to slow down; bills are already covered by Free to use, so none is skipped.

Contha does not move the money. The reader makes the transfers at their bank; a goal's progress follows its linked account's balance on the next sync.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
