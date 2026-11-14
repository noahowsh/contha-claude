---
name: contha-bill-cleanup
description: Use when the reader wants to review subscriptions and recurring charges, find bills they forgot about, check what is coming out of their account soon, or tell Contha that a subscription was cancelled or a charge is not really a bill.
license: MIT
---

# Bills and subscriptions cleanup

Lists every recurring charge Contha expects, helps the reader decide what stays, and records what they cancelled so Free to use is right.

## Steps

1. Call `get_overview` and note Free to use, so you can say later how it changed.
2. Call `get_subscription_review`. Each stream has a `key`, merchant, kind (bill or subscription), amount, cadence, cost a year, next date, any `priceChange`, and `flags`; the result also gives the monthly subscription total.
3. Group them: housing and utilities, insurance, debt, subscriptions and software, everything else. Give each stream's cost a year from the tool, and the tool's monthly subscription total.
4. Flag for the reader, without deciding for them:
   - Anything the tool flags: a price rise, a likely duplicate, an annual renewal soon, new this quarter.
   - Two charges from services that do the same thing.
   - Subscriptions they may not recognise; offer to look up past charges with `get_transactions` and `query`.
5. When the reader says they cancelled something, or that a row is not a bill (a one-off that repeated by chance), show the change and on a yes call `mark_bill` with its `key` and `cancelled` or `ignore`. Use `keep` to undo.
6. Call `get_overview` again and give Free to use before and after, both as the tool gave them.

Contha cannot cancel a subscription with the company. Say where the reader cancels it (the company's account page or their card's subscription settings) and mark it in Contha after.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
