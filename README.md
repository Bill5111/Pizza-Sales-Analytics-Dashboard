## Pizza Sales Analytics — 2015 Performance Dashboard

### Turning a full year of transactional pizza sales into clear, decision-ready insights on menu, timing, and customer behaviour.

### Tools used: Power Query (cleaning & transformation) · Power BI Desktop (snowflake data model, DAX, interactive multi-page report)
---

### 1. Purpose of the Project:

A pizza restaurant generates thousands of transactions a year, but raw order logs don't tell an owner what to cook more of, when to staff up, or which menu items to cut. Therefore, this project transforms 48,000+ raw 2015 transaction records into an interactive, two-page Power BI dashboard that answers the questions a business actually acts on:

> **Which products, days, hours, sizes, and ingredients drive the business — and where is money being left on the table?**

The result is a single source of truth for menu optimisation, staffing decisions, and promotional planning — moving the business from gut feel to evidence.

### 2. Key Insights:

#### Headline KPIs (2015)

| Metric | Value |
| :---: | :---: |
| Total Revenue | ~ $817.86K |
| Total Quantity Sold | ~ 50,000 pizzas |
| Total Orders | ~ 21,000 orders |
| Average Order Value | $38.31 |
| Average Pizzas per Order | 2.32 pizzas |

#### What the data revealed
- Timing is highly predictable. Friday is the busiest day (3,538 orders) and Sunday the weakest (2,624) — a 35% gap. Demand peaks at 12 PM (lunch rush) with a secondary peak at 6–7 PM, and goes quiet from 2–3 PM and after 9 PM.
- Seasonality occurs. Revenue peaks in July, October, and November and bottoms out in March and December. 
- "Classic" is the workhorse category. It leads on both quantity (14,888 units) and revenue ($220.6K). Meanwhile, Veggie ranks last on revenue ($193.7K).
- Premium pricing power exists. Thai Chicken ranks only 5th in units sold but 1st in revenue ($43.4K) — customers pay up for it. Chicken pizzas dominate the top-revenue list.
- Customers prefer mid-to-large sizes. Large dominates (~19K units), Medium ~16K, Small ~14K, while XL (~1K) and XXL (near zero) are negligible.
- Garlic is the winning ingredient — it appears across all top-selling pizzas, while the least popular ingredients cluster in the worst performers. There's a clear negative price-volume relationship across categories.
- Most orders are baskets, not singles. 62% are multi-item orders vs 38% single-item — and that 38% is the biggest untapped upsell opportunity.

### 3. Dataset Description:
Source: Plato's Pizza transactional sales records for the full 2015 calendar year.

Grain: One row per pizza line item (type + size) within an order — 48,620 rows, rolling up to 21,350 orders and 49,574 pizzas.

#### Variable dictionary
| Field | Description |
| :---: | :---: |
| order_id | Unique identifier for each order placed by a table |
| date | Date the order was placed (entered into the system prior to cooking & serving) |
| time | Time the order was placed (entered into the system prior to cooking & serving) |
| quantity | Quantity ordered for each pizza of the same type and size |
| pizza_id | Unique identifier for each pizza (constituted by its type and size) |
| size | Size of the pizza (Small, Medium, Large, X Large, or XX Large) |
| price | Price of the pizza in USD |
| name | Name of the pizza as shown in the menu |
| category | Category that the pizza fall under in the menu (Classic, Chicken, Supreme, or Veggie) |
| ingredients | Comma-delimited ingredients used in the pizza as shown in the menu (they all include Mozzarella Cheese, even if not specified; and they all include Tomato Sauce, unless another sauce is specified) |

#### Data preparation & modelling

The raw flat table was cleaned and reshaped in Power Query, then modelled into a snowflake schema in Power BI for efficient, reusable analysis:
- Cleaning — data types verified (dates, numerics, text), duplicates removed, and index-based primary keys added before normalisation.
- Fact_Pizza — transactional facts (`order_id`, `date`, `pizza_id`, `quantity`, `price`); redundant descriptive columns removed after merging.
- Dim_Pizza — `pizza_id` (PK), name, category, size, ingredients.
- Dim_Date — date (PK) with derived Month and Weekday for time-intelligence.
- Dim_Category — distinct categories acting as a hub for cross-table filtering.
- Ingredient_Sales — built by splitting comma-separated ingredients into rows and aggregating quantity sold per ingredient.
- Derived fields — Hour, Weekday, Month, Order Type (Single/Multi-item), and a `size_sort_order` helper (S=1 … XXL=5) so sizes sort logically rather than alphabetically.
- Measures — one-to-many relationships from each dimension to the fact table, plus 5 DAX measures powering the KPIs.

### 4. Visualisation:

The report has two linked, fully interactive pages. Shared Date and Category slicers filter every visual, and navigation buttons move between pages.

For more information, please navigate to [Pizza Sales Dashboard](https://app.powerbi.com/reportEmbed?reportId=c7b91bd6-5414-4bd5-8763-efce89f8251d&autoAuth=true&ctid=d02378ec-1688-46d5-8540-1c28b5f470f6)

#### Page 1 — Sales Overview

A top-line view of how the business performed over 2015: five KPI cards, a daily revenue trend line across the year, revenue by category, quantity-vs-price-per-unit by category, and order distributions by day of week and by hour.

![Pizza Sales Analytics — Sales Overview dashboard](images/Sales_Overview.png)

#### Page 2 — Product

A deep dive into which products and ingredients win or lose: top and bottom 5 pizzas by revenue and by quantity, most/least popular ingredients, an order-size habit donut (multi- vs single-item), and quantity sold across pizza sizes broken down by category.

![Pizza Sales Analytics — Product dashboard](images/Product.png)

### 5. Recommendations:

Three evidence-based actions, ordered by speed-to-impact:
- Discontinue or reformulate the Brie Carre Pizza. At 490 units and $11.6K revenue, it sits 5× below the top performer, and its signature ingredients have near-zero mass-market appeal. Replacing it with a product built on proven ingredients (Garlic, Mozzarella, Tomato Sauce) would lift category efficiency and simplify procurement.
- Run targeted seasonal promotions in March and December. Both months consistently underperform — December likely loses share to holiday dining and shopping habits. A "Christmas discount" or similar program could recover demand during these troughs.
- Convert single-item orders through checkout upselling. With ~8,100 single-item transactions (38% of all orders), an "add a second pizza at a discount" prompt is the highest-ROI short-term lever available — nudging customers toward the basket behaviour the other 62% already show.

For any queries, please reach out to phingochai2005@gmail.com
