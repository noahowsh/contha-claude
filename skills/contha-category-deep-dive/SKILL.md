---
name: contha-category-deep-dive
description: Use when the reader asks how much they spend on one thing ("how much do we spend on groceries", "what did Amazon cost us this year", "am I spending more on gas"), wants it month by month, by merchant, or by person.
license: MIT
---

# Category deep dive

One category or merchant, fully: the total, the trend, where it goes, and who spends it.

## Steps

1. Work out what the reader means. If it is a category, call `get_categories` to find its exact name. If it is a merchant, you will search by `query`.
2. Call `get_spending_trends` for the period (default this year) and take that category's row: total, monthly average, the latest month against its usual, and `byMonth`. The tool lists only the 25 largest categories and 15 largest merchants; if the one asked about is not there, use step 3 instead.
3. Call `get_transactions` with the dates, `query` (a merchant) and `limit: 500` to list the charges. If its `count` is more than `returned`, the list is cut short: say so, and do not total the list yourself; for a category, read the category on each row. Name the biggest few and the most frequent merchants within it.
4. For a household, call `get_spending_by_person` with the `category` for who spent it.
5. Answer in four lines: the total and average a month; the trend (rising, steady, falling, from `byMonth`); the top merchants; and, if asked, by person.
6. If the reader wants to rein it in, offer a budget line with `set_budget` at a figure they choose, and show the latest month's figure next to it.

If the category has a lot of uncategorized lookalikes (`get_transactions` with `needsReviewOnly: true` and the merchant as `query`), say so: the total is only as good as the filing.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
