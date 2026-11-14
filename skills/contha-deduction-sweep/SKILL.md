---
name: contha-deduction-sweep
description: Use when a self-employed reader wants to find missed business deductions, clean up their Schedule C, check for business expenses filed as personal, or work through transactions waiting for a category before taxes or a quarterly payment. Business plan. Proposes every change and files only what the reader approves.
license: MIT
---

# Deduction sweep

Finds business expenses sitting on the personal side, and uncategorized transactions, and files the ones the reader approves. Every one found lowers the tax estimate and makes the personal books truer.

## Steps

1. Call `audit_misfiled_business` for this year. It lists known business vendors in personal categories, largest first, each with its transaction `id`, current `category`, date, merchant and `readable` amount.
2. Call `get_transactions` with `needsReviewOnly: true` and `limit: 100` for the transactions still waiting for any category. These are a second list, not the misfiled ones.
3. Call `get_categories`. Each business category says its Schedule C line and whether it is fully or half deductible. Use only these ids.
4. Build one table, largest first: date, merchant, amount, where it is now, the category you suggest, and why in a few words. Mark anything you are unsure of as "ask" rather than guessing; meals, phones, home internet and vehicles are often partly personal.
5. Ask which rows to file. Accept "all", "all but 3 and 7", or row numbers.
6. File the approved rows with `file_transactions` (up to the tool's batch size, grouped by category, `side: "business"` where it moves to the business books).
7. For a merchant that showed up three or more times, offer a rule with `create_rule` so its future charges file themselves. Show the rule first.
8. Call `get_schedule_c` and report `form.line28TotalExpenses` and `form.line29TentativeProfit`, and the `lines` that changed, by their readable figures. If the reader wants the tax effect, call `get_tax_estimate` and give the new figure next to the old one.

## Do not

- Do not file anything the reader did not approve, and do not invent a business purpose.
- Do not move a charge to business only because the merchant could be business. Ask.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
