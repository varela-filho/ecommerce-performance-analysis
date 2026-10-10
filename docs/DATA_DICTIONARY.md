# Nordmarkt | Data Dictionary

All datasets are synthetic UTF-8 CSV files with headers. Currency is EUR. Empty fields represent missing values unless otherwise documented.

## `customers.csv` — customer dimension
**Expected grain:** one row per customer. **Primary key:** `customer_id`.

| Column | Meaning |
|---|---|
| `customer_id` | Customer identifier (text) |
| `signup_date` | Account creation date (YYYY-MM-DD) |
| `region` | Customer's recorded German region; some inconsistent/missing values |
| `country` | Customer's recorded country code |
| `acquisition_source` | Channel attributed at signup; **not** necessarily the channel of a later order |
| `customer_type` | Consumer or small-business account |
| `email_opt_in` | Recorded email marketing consent flag |

## `orders.csv` — order header
**Intended grain:** one row per order; inspect duplicates. **Logical key:** `order_id`.

| Column | Meaning |
|---|---|
| `order_id` | Order identifier |
| `customer_id` | Reference to `customers.customer_id` |
| `order_timestamp` | Order placement timestamp, local business time, `YYYY-MM-DD HH:MM:SS` |
| `channel` | Attributed order acquisition channel; may have formatting/missing issues |
| `device` | Device type used to place the order |
| `order_status` | `completed`, `cancelled`, or `returned` |
| `order_discount_pct` | Order-level discount percentage, 0–100; separate from item discount |
| `shipping_method` | Standard or express |
| `shipping_country` | Destination country code |

## `order_items.csv` — order lines
**Expected grain:** one row per order line. **Primary key:** `order_item_id`.

| Column | Meaning |
|---|---|
| `order_item_id` | Order line identifier |
| `order_id` | Reference to `orders.order_id` |
| `product_id` | Reference to `products.product_id` |
| `quantity` | Number of units in the line |
| `unit_price_eur` | Transaction unit price **before** line discount; some missing/invalid entries |
| `unit_discount_eur` | Discount per unit in EUR (not a percentage) |

## `products.csv` — product dimension
**Expected grain:** one row per product. **Primary key:** `product_id`.

| Column | Meaning |
|---|---|
| `product_id` | Product identifier |
| `product_name` | Display name |
| `category` | Product category |
| `brand` | Product brand |
| `list_price_eur` | Catalog/reference price, EUR |
| `unit_cost_eur` | Approximate unit cost, EUR |
| `launch_date` | Recorded launch date |
| `active_flag` | Product active indicator, 1/0 |

## `marketing.csv` — daily channel marketing activity
**Expected grain:** one row per `date` and `channel`.

| Column | Meaning |
|---|---|
| `date` | Activity date, YYYY-MM-DD |
| `channel` | Acquisition channel |
| `spend_eur` | Recorded marketing expenditure, EUR |
| `campaign_count` | Count of recorded campaigns |

**Caution:** Spend is a channel/day aggregate. Do not claim customer-level attribution or incremental return on ad spend without supporting evidence.

## `website_sessions.csv` — website traffic observations
**Expected grain:** one row per session; inspect duplicates. **Logical key:** `session_id`.

| Column | Meaning |
|---|---|
| `session_id` | Website session identifier |
| `session_date` | Session date, YYYY-MM-DD |
| `channel` | Attributed traffic channel |
| `device` | Session device type |
| `region` | Recorded region |
| `pages_viewed` | Pages visited in the session |
| `session_duration_seconds` | Session duration in seconds |
| `visitor_type` | New or returning visitor label |

**Important limitation:** There is no `session_id` on `orders.csv`, and there is no `customer_id` on `website_sessions.csv`. Session-level conversion cannot be reconstructed; aggregate order-to-session ratios may be used as **proxies** with explicit caveats.

## Relationships

```text
customers (customer_id) 1 ── * orders (customer_id)
orders (order_id)       1 ── * order_items (order_id)
products (product_id)   1 ── * order_items (product_id)

marketing (date, channel) ── aggregate comparison ── orders (order date, channel)
website_sessions (session_date, channel) ── aggregate comparison ── orders (order date, channel)
```

## Analysis notes
- Preserve raw files unchanged; clean into a separate working layer.
- Treat IDs as strings, even when they contain digits.
- Before joining orders to order items, decide how to handle duplicate order headers to prevent revenue inflation.
- Don't merge daily marketing directly onto order lines without controlling aggregation grain.
- Not every data issue should be silently deleted; explain its possible impact.
