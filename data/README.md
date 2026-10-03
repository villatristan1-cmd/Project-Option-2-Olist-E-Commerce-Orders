# Olist Project Data Guide

The provided files have different row meanings. Check keys before joining and keep the final ABT unit explicit.

| File | One row represents | Expected key | Integration note |
|---|---|---|---|
| `orders.csv` | One order | `order_id`; `customer_id` also identifies one order-specific customer record | Recommended base table for a one-row-per-order ABT |
| `customers.csv` | One order-specific customer record | `customer_id` | Join to orders on `customer_id`; `customer_unique_id` can repeat across orders |
| `order_items.csv` | One numbered item line within an order | `order_id` + `order_item_id` | Enrich with product and seller fields, then aggregate to `order_id` |
| `order_payments.csv` | One payment transaction or method sequence within an order | `order_id` + `payment_sequential` | Aggregate to `order_id` before joining |
| `order_reviews.csv` | One review record associated with an order | Review records are not guaranteed to be unique by `order_id` or `review_id` alone | Aggregate to `order_id` before joining |
| `products.csv` | One product | `product_id` | Join to item-level data before order-level aggregation |
| `sellers.csv` | One seller | `seller_id` | Join to item-level data before order-level aggregation |
| `geolocation_zip_prefix.csv` | One curated zip-code-prefix summary | `geolocation_zip_code_prefix` | Safe for a many-to-one join from customer or seller zip prefix |
| `product_category_name_translation.csv` | One Portuguese-to-English product-category mapping | `product_category_name` | Join to products before item/order aggregation |

Important target notes:

- Define `delivered_late` only for delivered orders with both actual and estimated delivery dates. An undelivered order is not automatically on time.
- Define a review-based target only when a review score is observed. A missing review is not automatically a low score.
- For predictions made at purchase time, exclude post-purchase fields such as actual delivery dates, final order status, and review outcomes.
