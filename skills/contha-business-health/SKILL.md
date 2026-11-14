---
name: contha-business-health
description: Use when a sole proprietor or freelancer asks how their business is doing, about profit margins, which months were best, where business money goes, or what their business is really earning. Business plan.
license: MIT
---

# Business health check

A plain read of how the business is doing this year: what it makes, what it keeps, and where the money goes.

## Steps

1. Call `get_profit_and_loss` for this year: money in and out by category, month by month, and what was left.
2. Call `get_schedule_c` for `form` (gross receipts, cost of goods, gross profit, total expenses, tentative profit).
3. Call `get_spending_trends` with `scope: "business"` (if the result has a `planNote`, the reader is not on the Business plan; stop and say this skill needs it) for the business's top merchants and categories against their usual.
4. Call `get_overview` for `safeToPayYourself` and `tax`.
5. Write:
   - **The year so far**: receipts, expenses and profit from `form`. Profit margin: tentative profit divided by gross receipts, with the division shown.
   - **Month by month**: the best and slowest months by profit, from the P&L.
   - **Where it goes**: top business expense categories and merchants; anything running above its usual.
   - **Cash**: safe to pay yourself, and the next tax payment.
6. Offer next steps that the numbers point to: a deduction sweep if expenses look light, the business card picker (`get_card_recommendations` with `book: "business"`) if business spending is large, or the quarterly taxes skill if a payment is near.

Do not benchmark against other businesses; there is no data for it here.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
