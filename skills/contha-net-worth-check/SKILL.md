---
name: contha-net-worth-check
description: Use when the reader asks about their net worth, what they own and owe, how their investments are doing, or wants a balance sheet. Explains net worth from cash, investments and debt.
license: MIT
---

# Net worth check

What the reader owns and owes, and what is moving it.

## Steps

1. Call `get_net_worth` for the total and its parts: cash, investments, property and debt.
2. Call `get_accounts` for each account's balance, type and how fresh its sync is (`staleDays`).
3. If they hold investments, call `get_holdings` for each position's market value and unrealized gain.
4. Present:
   - **Net worth** and its parts, as the tool gives them.
   - **By account**: a short table grouped as cash, investments, debts; flag any account whose sync is more than a few days old, since its balance may be out of date.
   - **Investments**: the largest positions and their unrealized gain or loss.
   - **Debt**: each card and loan balance.
5. If debt includes card balances carried month to month, offer the debt payoff skill. If cash is high and earning little, offer the idle cash skill.

Do not give investment advice or forecasts. Holdings and gains are as of their `asOf` date.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
