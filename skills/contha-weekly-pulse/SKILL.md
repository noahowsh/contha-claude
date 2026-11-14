---
name: contha-weekly-pulse
description: Use for a quick weekly money check ("how was this week", "Sunday money check", "am I on track this month"). A fast, five-line read of the last seven days against the month's pace.
license: MIT
---

# Weekly pulse

Five lines, once a week: what the last seven days cost, whether the month is on pace, and what is coming next week.

## Steps

1. Call `get_cashflow` with `from` seven days ago and `to` today. Take spending and its top two `spendByGroup` entries.
2. Call `get_budget` for this month for what is left overall and any line already over.
3. Call `get_upcoming_bills` with `days: 7`.
4. Call `get_overview` for Free to use and `needsAttention`.
5. Write exactly:
   - **This week**: spending, and the two biggest groups.
   - **The month**: budget left, or Free to use if there is no budget.
   - **Over**: any budget line already over, or "nothing over".
   - **Next week**: bills due, by date.
   - **To do**: transactions to categorize, if any, or an account not syncing.

No commentary beyond the five lines unless the reader asks.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
