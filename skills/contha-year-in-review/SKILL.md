---
name: contha-year-in-review
description: Use in December or January, or when the reader asks how their year went with money, for an annual recap, a year-over-year comparison, or their "money wrapped". Builds a year review from their books.
license: MIT
---

# Year in review

The reader's money year in one page: where it came from, where it went, what they kept, and what changed.

## Steps

1. Call `get_cashflow` for the year (`from` January 1, `to` December 31 or today). Read the totals, `months`, `incomeBySource` and `spendByGroup`.
2. Call `get_spending_trends` for the year: the categories by total and `topMerchants`.
3. Call `get_net_worth` and `get_goals`.
4. If the reader wants a comparison and the books go back that far, call `get_cashflow` for the year before.
5. On the Business plan, call `get_profit_and_loss` for the year and `get_schedule_c` for `form.line29TentativeProfit`.
6. Write, as short sections:
   - **The year**: income, spending, kept, and the savings rate with the division shown.
   - **Where it went**: top five spending groups and the top five merchants, with totals and charge counts.
   - **The best and worst months**: by what was left, from `months`.
   - **Goals and net worth**: where each goal ended, and net worth today.
   - **Business** (Business plan): profit for the year, tentative profit on Schedule C.
7. Close with three questions for next year that the numbers raise, not resolutions.

Quote figures exactly. If the books start partway through the year, say from when.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
