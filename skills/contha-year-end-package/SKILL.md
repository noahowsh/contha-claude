---
name: contha-year-end-package
description: Use when the reader is getting ready for tax season, wants to send their accountant or CPA their numbers, asks for a Schedule C summary, a year-end export, a mileage log, or a 1099 contractor list. Business plan. Checks the books first, then pulls the same files the app exports.
license: MIT
---

# Year-end package for the accountant

Gets the year's books ready to hand over: check what is unfinished, then pull the files and a one-page summary.

## Steps

1. Pre-flight. Call `get_overview` and `get_transactions` with `needsReviewOnly: true`. List what is unfinished: transactions still to categorize, accounts not syncing. Call `audit_misfiled_business` for the year. If anything is open, say so and offer the deduction sweep first; a package built on unfiled transactions is wrong.
2. Call `get_schedule_c` for the year and `get_profit_and_loss` for the year.
3. Pull the files the reader wants with `get_year_end_package` (`kind`: `schedule-c`, `transactions`, `mileage`, `1099`, `pnl`; `year`). Each comes back as CSV text whose first line names the file; the rest is the same bytes as the app's export. Where Claude can write files (Claude Code, or a chat with file creation on), save each one under that name; otherwise tell the reader the same files download from the app's Business page.
4. Write a one-page summary for the accountant:
   - Schedule C Part I and totals from `get_schedule_c`'s `form`: line 1 gross receipts, line 2 returns, line 4 cost of goods, line 5 gross profit, line 28 total expenses, line 29 tentative profit, each by its `readable` figure, with the notes the tool attaches. Then each expense line from `lines` with `readableSpent` and `readableDeductible`; where the two differ (meals are half deductible), say so.
   - Estimated payments made this year, from `get_quarterly_schedule`, federal and state separately (each row's `agencies`): an accountant enters them on different forms.
   - If the P&L's money out and Schedule C's line 28 differ, say why from the tools: line 28 includes the mileage deduction and leaves out cost of goods and returns, which sit above it in Part I.
   - Anything open or uncertain: uncategorized transactions, accounts not syncing, partly personal costs the reader should confirm.
   - For a seller: Schedule C line 1 comes from the 1099-K or the marketplace's own statement, which is usually larger than deposits because fees and refunds come out before the money lands. Say so.
5. Remind the reader the app can also give their accountant a read-only link (Business page, Accountant link), so the accountant sees the same books without a login.

## Do not

Do not round, re-add or re-categorize figures in the summary. Do not state what the reader owes for the year as fact; it is the accountant's call.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
