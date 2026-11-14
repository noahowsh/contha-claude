---
name: contha-budget-tune-up
description: Use when the reader has a budget that keeps breaking, wants to optimize or rebalance their budget, asks which budget lines are unrealistic, or wants to move money between lines. Compares each line with what they actually spend and proposes changes.
license: MIT
---

# Budget tune-up

Fixes a budget that does not match real life: lines planned too low (always broken), too high (money sitting idle), and spending with no line at all.

## Steps

1. Call `get_budget` for this month and for each of the last three months (`month: "YYYY-MM"`). Note, per line, how often it went over and by how much. A budget's `spent` counts budgeted lines only; spending in a category with no line is not in it.
2. Call `get_spending_trends` for the same months for each category's monthly average, including categories with no budget line.
3. Call `get_upcoming_bills` with `days: 30` to make sure fixed lines cover the bills that land in them.
4. Sort lines into:
   - **Always over**: over plan in most of the months. Propose the plan near the category's average, or a cut the reader is willing to make.
   - **Never close**: under 60% used every month. Propose lowering it and moving the slack to a line that keeps breaking, or to a goal.
   - **Missing**: a category with steady spending and no line.
   - **Lumpy**: costs that come a few times a year; propose `non_monthly` with `rollover: true`.
5. Show one table: line, planned now, average actual (tool figures), proposed, why. Keep the total planned equal to or under a typical month's income: the middle of the last three months from `get_cashflow`, since paychecks land unevenly. Say how it compares, showing the sum.
6. On a yes, call `set_budget` for each changed line, then `get_budget` to read it back.

A budget the reader will follow beats a tight one they abandon. When in doubt, propose the realistic figure and a separate, smaller cut.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
