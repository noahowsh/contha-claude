---
name: contha-money-check-in
description: Use when the reader asks how they are doing with money, wants a weekly or monthly check-in, asks "where do I stand", "what can I spend", or "anything I should deal with". Reads their Contha books (personal, and business on the Business plan) and leads with the one thing to do next.
license: MIT
---

# Money check-in

A short, honest read of where the reader stands today, from their own books, ending with the one next move.

## Steps

1. Call `get_overview`. It returns Free to use with the sum behind it, net worth, this month's income and spending (each book apart on the Business plan), bills coming up, the next tax payment and the reserve, safe to pay yourself, and what needs attention.
2. Call `get_upcoming_bills` with `days: 14` for the bills in the next two weeks.
3. If `needsAttention.transactionsToCategorize` is above zero, call `get_transactions` with `needsReviewOnly: true` and `limit: 10` so you can name a few of them.
4. If the reader asked about a period other than this month, call `get_cashflow` with `from` and `to` for that period instead of reading `thisMonth`.
5. Write the check-in in this order, each a sentence or two:
   - **Free to use**: the amount and the `how` sentence, verbatim. If `short` is set, say they are short by that amount and that it comes before anything else.
   - **This month**: income against spending, for each book on the Business plan. Name the top two spending groups.
   - **Coming up**: the bills from `get_upcoming_bills`, by date, and their total.
   - **Tax** (Business plan): `tax.dueNext`, any `pastDue`, and whether the reserve covers it.
   - **Needs you**: transactions to categorize and accounts not syncing, by name.
6. End with **Next:** one action, the most valuable one. Prefer, in order: a past-due tax payment, being short, an account not syncing, a pile of uncategorized transactions.

## Style

Plain sentences, the reader's own numbers, no pep talk. Under 200 words unless they ask for more. Offer, once, to run a deduction sweep or a budget review if the numbers point to one.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
