---
name: contha-budget-builder
description: Use when the reader wants to make a budget, set spending targets, fix a budget that never matches reality, or review how this month's budget is going. Builds lines from their real spending and sets them only after they agree.
license: MIT
---

# Budget builder

Builds a monthly budget from what the reader actually spends, then sets it once they agree. Also reviews a month already budgeted.

## Review an existing budget

1. Call `get_budget` for the month (default this month) and `get_upcoming_bills`.
2. Report lines over plan, lines above 80% used, what is left overall, and which bills still to come this month land in which lines. Stop here unless they want changes.

## Build a new one

1. Call `get_cashflow` for the last three full months. Use its spending by group and month by month series.
2. Call `get_categories` for expense category ids.
3. Propose a table: category, average a month (from the tool's monthly figures), proposed plan, and type: `fixed` for bills that do not change, `flexible` for everyday spending, `non_monthly` for lumpy costs like car insurance or gifts. Round plans to a sensible figure and say you rounded.
4. Point out any category where one month was far off the others; ask whether it was a one-off.
5. Ask the reader to approve or adjust. When they agree, call `set_budget` once per line (`categoryId`, `plannedCents`, `variability`, `month`). Offer `rollover` for non-monthly lines so unspent money carries forward.
6. Call `get_budget` to read the result back.

Keep the first budget to the eight to twelve categories that matter. A budget with forty lines does not get used.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
