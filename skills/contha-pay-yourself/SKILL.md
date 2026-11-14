---
name: contha-pay-yourself
description: Use when a sole proprietor or freelancer asks how much they can pay themselves, take out of the business, or move from business to personal this month without coming up short on taxes or business bills. Business plan.
license: MIT
---

# Pay yourself

How much a sole proprietor can safely move from the business to personal, and why.

## Steps

1. Call `get_overview`. Read `safeToPayYourself`, `tax` and the business book in `thisMonth`.
2. Call `get_upcoming_bills` with `days: 30` and pick out the business bills.
3. Call `get_quarterly_schedule` for the next estimated payment and whether anything is past due.
4. Answer:
   - The safe amount, as `get_overview` gives it, and that it already holds back the tax reserve and business bills ahead.
   - If tax is past due or the reserve is short, say that comes first and the safe amount reflects it.
   - If `safeToPayYourself` is missing, the business has no cash account linked; say so.
5. If the reader wants a steady monthly draw, call `get_profit_and_loss` for the year and show the business's monthly profit series from the tool so they can pick a figure the business supports in a slow month, not an average one.

Contha does not move the money. The reader makes the transfer at their bank; it will show as a transfer, not as spending or income.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
