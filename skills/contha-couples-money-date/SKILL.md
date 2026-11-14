---
name: contha-couples-money-date
description: Use when a couple wants to sit down and go over money together, asks for an agenda for a monthly money talk, or wants a fair, shared view of household spending, bills and goals.
license: MIT
---

# Couples money date

A calm, shared agenda for a monthly money conversation, built from the household's books so nobody argues from memory.

## Steps

1. Call `get_overview`, `get_spending_by_person` for last month, `get_upcoming_bills` with `days: 30`, `get_goals`, and `get_budget` for this month.
2. Write the agenda in this order:
   - **Where we stand**: Free to use and its `how` sentence.
   - **Last month, together**: spending by person as a table, with Joint shown. A sentence of fact, no verdict.
   - **What is coming**: the bills in the next 30 days, by date.
   - **Our goals**: each goal's progress and whether it is behind.
   - **The budget**: lines over, lines with room.
   - **Decisions for this month**: two or three questions the numbers raise (for example, "the trip goal is behind by its monthly need; push the date or add to it?").
3. After they talk, record what they decide: a budget change with `set_budget`, a purchase marked as one person's with `file_transactions` and `whose`, a subscription they cancelled with `mark_bill`. Show each first.

## Tone

Neutral and even. Present both people's figures the same way. Never suggest who should spend less; ask what they want to decide.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
