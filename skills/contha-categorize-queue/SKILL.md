---
name: contha-categorize-queue
description: Use when the reader has transactions waiting for a category, wants to clear their review queue, tidy up categories, or set up rules so the same merchants file themselves next time. Works on both Personal and Business plans.
license: MIT
---

# Clear the categorize queue

Works through transactions waiting for a category in batches, files what the reader approves, and adds rules so the queue stays short.

## Steps

1. Call `get_transactions` with `needsReviewOnly: true` and `limit: 50`.
2. Call `get_categories` and `get_rules`. Use only real category ids; do not suggest a rule that already exists.
3. Group the queue by merchant. For each group propose a category (and on the Business plan, a side: business or personal) with a few words of reason. Mark anything ambiguous as "ask".
4. Show the batch as a table, numbered, and ask for approval ("all", "all but 4", or row numbers, or changes).
5. File approved rows with `file_transactions`, grouped by category. For a single transaction whose merchant will recur, `rememberForMerchant: true` makes a rule as it files.
6. For merchants with three or more rows, offer a rule with `create_rule` (`merchant` exactly as shown, `categoryId`, `side`, and `sign: "in"` for deposits only, such as a client's payments). Show the rule first.
7. Repeat with the next batch until the queue is empty or the reader stops. End with how many are left, from `get_overview`.

Transfers between the reader's own accounts and card payments are not spending; file them to the transfer category, not an expense.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
