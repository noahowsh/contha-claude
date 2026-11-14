# Contha for Claude

Twenty-nine skills that turn Claude into a careful bookkeeper for your personal and business money, answered from your own [Contha](https://contha.com) books. Every figure comes from a Contha tool call, never from Claude's arithmetic, and nothing in your books changes until you have seen the change and said yes.

The plugin bundles the Contha connector (`https://app.contha.com/api/mcp`). You sign in to Contha the first time Claude uses it. You need a Contha account; the free plan works, no card.

## Skills

**Everyday money** (Personal and Business)

| Skill | Ask it |
|---|---|
| `contha-money-check-in` | "How am I doing this month?" |
| `contha-weekly-pulse` | "Quick Sunday money check." |
| `contha-month-in-review` | "How did October go?" |
| `contha-year-in-review` | "Give me my money year in review." |
| `contha-can-i-afford-it` | "Can I afford a $1,400 flight?" |
| `contha-paycheck-plan` | "I just got paid $3,200. Where should it go?" |
| `contha-windfall-plan` | "I got a $5,000 bonus. What do I do with it?" |

**Spending and budgets**

| Skill | Ask it |
|---|---|
| `contha-spending-leaks` | "Where is my money actually going?" |
| `contha-category-deep-dive` | "How much do we spend on groceries, month by month?" |
| `contha-cut-spending-plan` | "I need to free up $500 a month." |
| `contha-budget-builder` | "Build me a budget from what I actually spend." |
| `contha-budget-tune-up` | "My budget keeps breaking. Fix it." |
| `contha-categorize-queue` | "Clear my uncategorized transactions." |

**Bills and subscriptions**

| Skill | Ask it |
|---|---|
| `contha-bill-cleanup` | "What subscriptions am I paying for?" |
| `contha-price-hike-watch` | "Did anything go up? What renews soon?" |
| `contha-bill-negotiator` | "Help me get my internet bill down." |

**Cards, savings, goals and debt**

| Skill | Ask it |
|---|---|
| `contha-credit-card-picker` | "Which credit card fits my spending?" |
| `contha-idle-cash` | "Is my savings earning enough?" |
| `contha-emergency-fund` | "How many months could we last?" |
| `contha-goal-planner` | "Will we hit our house deposit by June?" |
| `contha-debt-payoff` | "Which card should I pay off first?" |
| `contha-net-worth-check` | "What's my net worth, and what moved it?" |

**Couples**

| Skill | Ask it |
|---|---|
| `contha-household-split` | "How much did we each spend eating out?" |
| `contha-couples-money-date` | "Make us an agenda for our monthly money talk." |

**Self-employed** (Business plan)

| Skill | Ask it |
|---|---|
| `contha-quarterly-taxes` | "What do I owe for my next estimated payment?" |
| `contha-deduction-sweep` | "Find business expenses I filed as personal." |
| `contha-pay-yourself` | "How much can I take out of the business?" |
| `contha-business-health` | "How is the business actually doing?" |
| `contha-year-end-package` | "Get my books ready for my accountant." |

## Install

**Claude Code**

```
claude plugin marketplace add noahowsh/contha-claude
claude plugin install contha@contha
```

**Claude (web and desktop)**: add the connector under Settings, Connectors, Add custom connector, with the address `https://app.contha.com/api/mcp`. To add a skill, zip its folder from `skills/` and upload it under Settings, Capabilities, Skills.

**Try it without an account**: add `https://demo.contha.com/api/mcp` as a connector instead. It reads a sample freelancer's books (`?plan=personal` for a couple, `?plan=shop` for an online shop) and refuses every change.

## What it can and cannot do

- It reads your accounts, transactions, categories, rules, budgets, bills and tax figures, as you see them in the app.
- With write access, which you choose on Contha's approval page, it can file transactions, add cash transactions, delete ones you added by hand, add rules, set budgets and mark bills. Claude asks before each change.
- It never moves money, pays a bill, or contacts anyone.
- Tax figures are estimates from your transactions, not tax advice. Confirm exact amounts with your accountant before paying or filing.

## Privacy Policy

Contha's privacy policy is at https://contha.com/privacy. In short: the connector reads only the books of the Contha account you sign in with, under that account's own permissions. Contha does not store your conversations with Claude, does not sell your data, and does not use it to train models. A connection lasts 90 days and you can end it any time under Settings, Claude and ChatGPT in the app. Questions: privacy@contha.com.

## Support

support@contha.com, or https://contha.com/ai for setup help.

## License

MIT
