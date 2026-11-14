---
name: contha-price-hike-watch
description: Use when the reader asks whether any subscriptions or bills went up, what renews soon, about annual renewals, or wants to catch price increases before they are charged.
license: MIT
---

# Price hike and renewal watch

Catches what went up and what is about to renew, while there is still time to act.

## Steps

1. Call `get_subscription_review`. Look at each stream's `priceChange` and `flags` (price went up, annual renewal soon, looks like a duplicate, new this quarter), and `stillChargingAfterCancel`.
2. Build three short lists, each with figures from the tool:
   - **Went up**: merchant, old and new price, since when, and the new cost a year.
   - **Renewing soon**: annual charges with their next date and amount.
   - **Still charging after you cancelled**: merchant, last charge. These need a call to the company or the card issuer.
3. For each, give one action: keep, cancel with the company before the date, call to ask for the old price, or dispute with the card issuer for a charge after cancelling.
4. When the reader cancels something, offer `mark_bill` with the key and `cancelled`, so Contha stops counting it in Free to use and will flag it if it charges again.

Nothing here is urgent unless a renewal date is within a week; say which ones are.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
