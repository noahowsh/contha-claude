---
name: contha-spending-leaks
description: Use when the reader asks where their money goes, why they never have anything left, what they could cut, or wants to find small habits that add up (delivery, coffee, rideshare, impulse shopping). Finds the leaks in their real spending and sizes each one.
license: MIT
---

# Spending leaks

Finds the habits and creeping categories that quietly eat the month, sized from the reader's own transactions.

## Steps

1. Call `get_spending_trends` for this year to date. Read `categories` (each with its monthly average and how the latest month compares) and `topMerchants` (total, number of charges, cost per charge).
2. Call `get_subscription_review` for recurring charges and their `flags`.
3. Look for, in this order:
   - **Habits**: merchants with many small charges (`charges` high, `perCharge` under about $60). These are the ones people underestimate. Leave out rent, loans and other bills (`kind: "bill"` in `get_subscription_review`): they are fixed, not habits, and the bill negotiator skill is for those.
   - **Creep**: categories whose recent months run above their average.
   - **Recurring**: subscriptions with a price rise, a likely duplicate, or a renewal coming.
4. For the top three or four, call `get_transactions` with `query` set to the merchant (or the category's dates) to confirm it is what it looks like.
5. Present a short table: leak, what it cost this year (tool figure), how often, and one plain way to shrink it. Order by size.
6. Ask which ones the reader wants to act on. For a habit, offer a budget line with `set_budget`; for a subscription they cancel with the company, offer `mark_bill` with its key.

## Tone

Specific and unjudging. "Ramen Hana, 40 visits, $2,163 this year" is useful; "you eat out too much" is not. Never call a category a waste; the reader decides what is worth it.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
