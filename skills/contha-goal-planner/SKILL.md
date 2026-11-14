---
name: contha-goal-planner
description: Use when the reader asks how their savings goals are going, whether they will hit a goal on time, how much to save each month, or wants to plan toward something (a trip, a house deposit, a car, a wedding). Works from their goals and real spending.
license: MIT
---

# Goal planner

Checks each goal against its date, finds the monthly amount, and makes room for it in the budget.

## Steps

1. Call `get_goals`. Each goal shows target, date, saved from its linked accounts, what is left, what it needs each month to land on time, and whether it is behind.
2. Call `get_cashflow` for the last three full months for what is typically left over each month.
3. Compare the goals' monthly needs, leaving out a goal marked `taxReserve` (that money is the tax, which Free to use already holds back), against what is usually left over, subtracting step by step. If it fits, say so. If not, say by how much.
4. If it does not fit, offer the reader choices, not a verdict: push a date out, lower a target, or free up money with the cut-spending plan skill.
5. If the reader keeps a budget, offer to put each goal in it. A goal's budget line is the category named `Savings: <goal name>` (cut to 40 characters for a long name); call `get_categories` to find it, then `set_budget` with the goal's monthly need, after a yes. If the category is not there yet, the goal's card on the Goals page adds it to the budget in one step.
6. For a new goal, help them pick a target and date and work out the monthly figure (target minus saved, divided by months left, shown). They add it on the Goals page and link the account that holds the money.

A goal with no date has no monthly need; say so rather than inventing one.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
