# Example Brief: Weekly Competitor Price Report

A filled-in brief for a small online store that tracks competitor prices.

```text
Goal: Every Monday, tell me which of our 20 core products are priced more than 10% above
      the market, so I can decide on price changes before the weekly promo goes out.
Type: Automation
Method: First list the tools and steps a pricing analyst would use for this.
        Then use the same tools and steps. Do not role-play.
Priorities:
  1. Accuracy of competitor prices
  2. Clear recommendation per product
  3. Speed
Tools and data:
- Built-in browser: competitor product pages (public prices)
- Google Sheets "Core Products": our SKUs, current prices, cost
Good result: A table of SKU | our price | lowest competitor price | gap % | source link |
             recommendation. Every competitor price has a link and a date checked.
             A guessed or remembered price counts as a failure.
Fail: Comparing different pack sizes; using a sale price without labelling it.
Done when: All 20 products have a verified row, or a row marked "not found" with a reason.
Review loop: P6 fact-check on the price table, then P3 ×1 → P4 → P5 on the recommendations.
Needs my approval: Any change to prices in the spreadsheet.
Never: Log into competitor accounts; edit the cost column.
Principles:
- Prefer recommending no change over a small change.
- Flag products where the market moved more than 20% in a week.
Hand-over status: side-by-side round 1
```
