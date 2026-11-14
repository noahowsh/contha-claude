---
name: contha-cut-spending-plan
description: Use when the reader says they need to free up a specific amount ("I need $500 more a month", "we have to cut back", "how do I save for X faster") or wants a plan to reduce spending. Builds a ranked list of cuts from their own books that reaches the target.
license: MIT
---

# Cut-spending plan

Turns "I need to free up $X a month" into a specific list of cuts, from the reader's own spending, that adds up to it.

## Steps

1. Get the target a month. If the reader gave none, ask.
2. Call `get_spending_trends` for the last full three months, `get_subscription_review`, and `get_budget` for this month.
3. Build candidates, each with its monthly figure from the tools:
   - Subscriptions and recurring charges they could cancel or downgrade (from `streams`, especially any with `flags`).
   - Flexible categories running above their average.
   - High-frequency merchants (`topMerchants` with many charges).
   - Fixed bills that are often negotiable: phone, internet, insurance. Offer the bill negotiator skill for these.
4. Rank by how much each frees up and how easy it is. Propose cuts as concrete actions ("cancel X", "eating out from about $Y to $Z a month"), never as a percentage of a whole category.
5. Show the running total against the target, adding the monthly figures step by step. Stop when it is reached; say if it cannot be reached from spending alone.
6. On the reader's choices: `mark_bill` cancelled for anything they have cancelled with the company, and `set_budget` for the reduced lines.

Never cut rent, debt minimums, insurance or medical costs on the reader's behalf; name them as fixed unless the reader raises them.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
