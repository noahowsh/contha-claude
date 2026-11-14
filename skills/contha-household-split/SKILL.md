---
name: contha-household-split
description: Use when a couple or household asks who spent what, how much each person spent on something (eating out, groceries, Amazon), whether spending is even, what one owes the other, or wants to mark a purchase as one person's. Uses Contha's per-purchase "whose".
license: MIT
---

# Household split

Answers "who spent what" for a couple or household, and keeps each purchase marked with whose it was.

## How whose works in Contha

Each purchase belongs to someone: its own "whose" if one was set, else the owner of the account it was on, else "Not said". Joint accounts default to Joint. `get_accounts` shows each account's owner. Use one of those names, or Joint, spelled exactly as `get_accounts` gives it: the tool accepts any text, so a misspelling would start a new person in the split.

## Steps

1. Call `get_spending_by_person` for the period (`from`, `to`; default this month), with `category` if they asked about one ("Eating Out", "Groceries"). A negative figure for a person means refunds outweighed spending; say so rather than hiding it.
2. If they want the detail, call `get_transactions` with `whose` and the same dates. Leave out transfers and card payments when reading the rows (their category is a transfer); `get_spending_by_person` already does, so the rows will not sum to it otherwise.
3. Answer with a small table: person, amount, share of the total. Then one plain sentence on what stands out.
4. Settling up: only if the reader asks, and only with the rule they give (50/50, by income, or "I pay rent, you pay groceries"). Show the arithmetic from the tool's figures step by step so they can check it. Contha records; it does not move money between partners.
5. Fixing whose: when a purchase is marked wrong, show the rows and the change, wait for a yes, then call `file_transactions` with `ids` and `whose` (a name, `Joint`, or `account` to hand it back to the account's owner).

## Tone

Neutral. This is a ledger, not a verdict on either person. Never comment on who "should" spend less.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
