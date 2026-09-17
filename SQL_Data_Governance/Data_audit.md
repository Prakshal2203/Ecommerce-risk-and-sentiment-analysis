# 🗄️ Olist E-Commerce: SQL Data Governance & Auditing

**Objective:** 
Before connecting the Kaggle dataset to Power BI, rigorous data auditing and cleansing were performed in MySQL Workbench. This ensures all dashboard metrics—especially defect rates and delivery times—are built on a foundation of absolute data integrity.

---

## Phase 1: Structural Integrity Checks (Primary Keys)
**Business Logic:** Ensure there are no duplicate records in the core transactional tables. A duplicate `order_id` would double-count revenue and artificially inflate order volumes.

```sql
-- Check for duplicate Order IDs in the orders table
SELECT 
    order_id, 
    COUNT(order_id) as duplicate_count
FROM 
    olist_orders_dataset
GROUP BY 
    order_id
HAVING 
    COUNT(order_id) > 1;
    
-- Check for duplicate Seller IDs in the sellers table
SELECT 
    seller_id, 
    COUNT(seller_id) as duplicate_count
FROM 
    olist_sellers_dataset
GROUP BY 
    seller_id
HAVING 
    COUNT(seller_id) > 1;
```

## Phase 2: Referential Integrity (Foreign Keys)
**Business Logic:** Verify that all transactional data maps correctly to our dimensional tables. An order cannot exist without a valid customer, and an order item cannot exist without a valid seller.

```sql
-- Identify orphaned orders (orders linked to a customer not in the customers table)
SELECT 
    o.order_id, 
    o.customer_id
FROM 
    olist_orders_dataset o
LEFT JOIN 
    olist_customers_dataset c ON o.customer_id = c.customer_id
WHERE 
    c.customer_id IS NULL;

-- Identify order items linked to non-existent sellers
SELECT 
    oi.order_id, 
    oi.seller_id
FROM 
    olist_order_items_dataset oi
LEFT JOIN 
    olist_sellers_dataset s ON oi.seller_id = s.seller_id
WHERE 
    s.seller_id IS NULL;
```
## Phase 3: Data Quality & Null Audits
**Business Logic:** For the delivery and logistics analysis, we must isolate missing data. A null order_delivered_customer_date is expected for canceled or processing orders, but must be filtered out when calculating average delivery days.

```sql
-- Audit missing delivery dates across different order statuses
SELECT 
    order_status,
    COUNT(order_id) AS total_orders,
    SUM(CASE WHEN order_delivered_customer_date IS NULL THEN 1 ELSE 0 END) AS missing_delivery_dates
FROM 
    olist_orders_dataset
GROUP BY 
    order_status;
    
-- Audit missing review scores in the sentiment analysis table
SELECT 
    COUNT(review_id) AS total_reviews,
    SUM(CASE WHEN review_score IS NULL THEN 1 ELSE 0 END) AS missing_scores
FROM 
    olist_order_reviews_dataset;
```

## Phase 4: Data Type Standardization & Casting
**Business Logic:** To enable Power BI's Time Intelligence functions, string-based timestamps must be properly cast to DATETIME formats within MySQL prior to export.

```sql
-- Cast string dates to standardized DATETIME format
ALTER TABLE olist_orders_dataset
MODIFY COLUMN order_purchase_timestamp DATETIME,
MODIFY COLUMN order_delivered_customer_date DATETIME,
MODIFY COLUMN order_estimated_delivery_date DATETIME;

-- Verify casting success and identify chronological anomalies (e.g., delivery before purchase)
SELECT 
    order_id,
    order_purchase_timestamp,
    order_delivered_customer_date
FROM 
    olist_orders_dataset
WHERE 
    order_delivered_customer_date < order_purchase_timestamp;
```
