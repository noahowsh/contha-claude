---
name: contha-windfall-plan
description: Use when the reader gets extra money (a bonus, tax refund, gift, inheritance, a big client payment, a raise) and asks what to do with it. Puts it to work in a sensible order using their own books.
license: MIT
---

# Windfall plan

What to do with extra money, in an order that covers the risks first, using what the reader's books already show.

## Steps

1. Get the amount. Note whether it is taxed already (a bonus usually is; a big client payment on the Business plan is not).
2. Call `get_overview` (tax due, past due, Free to use), `get_accounts` (card and loan balances), `get_goals`, and `get_savings_opportunities`.
3. Propose an order, skipping any step that does not apply, with the figures from the tools:
   1. **Tax**: on the Business plan, any past-due estimated tax, then the reserve for the next payment (`get_quarterly_schedule`). Untaxed income needs its share set aside first.
   2. **High-interest debt**: card balances carried month to month (ask the rates). Paying a card does not raise Free to use, which already took the card off; it saves the interest.
   3. **Emergency fund**: up to the emergency goal's own `remaining` if there is one (otherwise offer the emergency fund skill to size it).
   4. **Other goals that are behind**: from `get_goals`, leaving out the emergency fund (step 3) and any goal marked `taxReserve` (step 1).
   5. **The rest**: the reader's choice; something for fun is part of a good plan.
4. Show the amount flowing down the list, subtracting step by step until it runs out.
5. If cash will sit in savings, point to the best rate from `get_savings_opportunities`.

Contha does not move money or invest it. Retirement accounts and investing are outside what these books can answer; say so if asked, and suggest a fee-only adviser for a large sum.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
