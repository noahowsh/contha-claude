---
name: contha-bill-negotiator
description: Use when the reader wants to lower a bill (phone, internet, cable, insurance, gym, streaming), asks if they are overpaying, or wants a script for calling a company to ask for a better price.
license: MIT
---

# Bill negotiator

Prepares the reader to call a company and ask for a lower price, with the facts from their own books.

## Steps

1. Call `get_subscription_review` and pick out bills that are commonly negotiable: internet, cable, phone, insurance, gym, satellite radio, security monitoring, and any with a price rise.
2. For the one the reader chooses, call `get_transactions` with `query` set to the merchant and `limit: 500` for the history. If `count` is more than `returned`, say the history is longer than shown.
3. Build a fact sheet from the tools: how long they have paid (first charge to last), the current amount, any rise and when, and the total paid so far (the tool's charges added, shown).
4. Write a short call script:
   - Open: they have been a customer since the first charge's date and the price has gone from X to Y.
   - Ask: for the current promotional or loyalty rate, or a cheaper plan with what they actually use.
   - If no: ask for the retention or cancellation department, and be ready to cancel.
   - Close: confirm the new price and when it starts, and ask for it in writing.
5. Remind them not to accept a contract or a longer term they do not want for a small discount.
6. After the call, if the price changed, there is nothing to record: Contha will pick up the new amount from the next charge. If they cancelled, offer `mark_bill` with `cancelled`.

Do not claim what a competitor charges or what the company "has to" offer. You do not know.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
