---
name: contha-quarterly-taxes
description: Use when a freelancer, sole proprietor or side-business owner asks what they owe for estimated taxes, when the next quarterly payment is due, whether they have set enough aside, or how a quarterly amount was worked out. Business plan. Not tax advice.
license: MIT
---

# Quarterly estimated taxes

Tells a sole proprietor what the next federal estimated payment is, when it is due, how it was worked out, and whether their tax reserve covers it.

## Steps

1. Call `get_quarterly_schedule` for this year. Each of the four rows has `due`, `status`, `setBy` (the rule that set it: the annualized income installment or a quarter of the required annual payment) and `readable` amounts: `installment` (this quarter's own share), `toPay` (what to send by this date to be current), and `requiredSoFar` and `paidSoFar`, which count from January through that quarter, as the IRS does. `dueNext.readable` is the next payment in one line.
2. Call `get_overview` for `tax.dueNext`, `tax.pastDue` and `tax.reserve`.
3. If the reader gives last year's total tax, last year's AGI, or payments already made that the books do not show, call `get_tax_estimate` with `priorYearTax`, `priorYearAgi` or `estPaymentsMade` (all in cents) and say how the answer changed. `get_tax_estimate` is the tax on the income so far this year (its `basis` says so); `get_quarterly_schedule` projects the whole year. Quote the schedule for what to pay, and the estimate only as the year to date, never as a refund or a balance for the year. `estPaymentsMade` replaces the payments on record: pass the full total paid this year, not only the part the books are missing. Over $150,000 of AGI the safe harbor is 110% of last year's tax; the tool applies it.
4. Answer in this order:
   - The next payment: amount, due date, and the method in one plain sentence. If the schedule's `next` is empty, every quarter is covered: quote `dueNext.readable` (often what is due with the return) and say so. Federal and state are paid to different places; say which is which from each row's `agencies`.
   - Anything past due, first and clearly, with the amount.
   - Whether the reserve covers it, or how short it is. If `tax.reserve` says `unset`, the reader has not chosen a reserve account; say Contha can track one if they keep tax money in its own account.
   - A small table of the four quarters: due date, installment, to pay, status, and `setBy`, from each row's `readable` amounts. If you show paid, label it "paid so far this year"; it is not that quarter's payment.
5. Close with what the estimate leaves out, from the tool's caveat (for example itemized deductions, credits and capital gains), and that it is Contha's estimate, not tax advice: the reader should confirm the exact amount with their accountant before paying. Payments are made at IRS Direct Pay or EFTPS and the state's site; Contha does not pay them.

## When the numbers look off

If a quarter looks too high, the usual cause is business income or a business expense filed on the wrong side. Offer the deduction sweep before the reader pays.

## Ground rules

- Every dollar figure you state comes from a Contha tool result in this conversation, quoted as the tool gave it; where a tool gives both cents and a `readable` figure, quote the readable one. Do not estimate or recall numbers. When this skill asks for a calculation, show each step with the tool's figures so the reader can check it, and label the result as yours, not Contha's.
- Read before you write. Before calling any tool that changes the books (`file_transactions`, `add_transaction`, `delete_transaction`, `create_rule`, `set_budget`, `mark_bill`), show the exact changes as a short table and wait for the reader to say yes. One yes covers the table you showed, nothing more.
- Write tools take amounts in cents (`-1250` is $12.50 spent). If a write is refused because the connection is read only, tell the reader to remove Contha in Claude's connector settings, connect it again, and tick "Also let it make changes" on Contha's approval page.
- Tax figures are Contha's estimates, not tax advice. Say so, say which method the tool used and what it leaves out, and suggest the reader confirm exact amounts with their accountant before paying or filing.
- Some tools are Business plan only. If a tool answers that it is on another plan, say so once and carry on with what the reader's plan has.
- No Contha tool moves money. If the reader asks you to pay, transfer, apply or cancel something with a company, say what Contha can record and that the action itself happens at their bank or with the company.
