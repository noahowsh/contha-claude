---
name: contha-can-i-afford-it
description: Use when the reader asks whether they can afford something, whether a purchase is OK right now, how much they can spend before payday, or if they can take money out for something. Answers from Free to use and the bills still to come.
license: MIT
---

# Can I afford it?

A straight answer to "can I afford this" from what is actually free, not from the bank balance.

## Steps

1. Get the price. If the reader did not give one, ask. Ask, too, if it is not obvious, whether it is what one of their goals is for (a trip, a car).
2. Call `get_overview`. Free to use is cash less what is owed on cards, cash already set aside for goals, the bills due in the next 30 days and, on the Business plan, tax due. Quote its `how` sentence.
3. If the purchase is what a goal is for, call `get_goals`: that goal's `saved` is money for exactly this, on top of Free to use. Never a goal marked `taxReserve`: that money is the tax. Compare the price with the goal's `saved` first, then any remainder with Free to use.
4. On the Business plan, Free to use counts personal and business cash together (its `covers` says so). For a personal purchase, also call `get_accounts` and name what the personal accounts (`isBusiness: false`) hold, so the answer does not lean on the business's money.
5. Call `get_upcoming_bills` with `days: 30` to name the large bills still ahead, and, if they have a budget for the category, `get_budget` for what is left in that line.
6. Answer:
   - **Yes** if the price is comfortably inside Free to use (plus the goal's money, when it is the goal's purchase) and the budget line, if any, has room. Say what will be left after it.
   - **Yes, but** if it fits but breaks a budget line, takes most of a goal, or leaves little cushion. Say which, as the tools give it.
   - **Not now** if it is more than that, or they are already short. Say what would change the answer (the next income, a bill that can wait) without inventing dates.
7. On the Business plan, if the reader means taking money out of the business, use `safeToPayYourself` from `get_overview` instead and say it already holds back the tax reserve and business bills.

Keep it to a few sentences. The reader asked a yes or no question.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
