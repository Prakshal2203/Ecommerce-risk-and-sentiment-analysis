# Olist E-Commerce: Business Performance & Risk Analytics

## Executive Summary
This project delivers an end-to-end business intelligence solution using the Brazilian E-Commerce dataset by Olist. The objective is to transform raw, anonymized transactional data into an actionable, executive-ready Power BI dashboard that diagnoses operational bottlenecks, assesses vendor risk, and tracks core revenue drivers. 

The analytical workflow encompasses strict data governance (via SQL) and advanced dimensional modeling and visualization (via Power BI), structured to answer strategic business questions regarding customer churn and logistical inefficiencies.

## Tech Stack & Methodology
* **Database Management:** MySQL Workbench (Data auditing, cleaning, referential integrity)
* **Business Intelligence:** Power BI Desktop (Data modeling, DAX measure creation, interactive visualizations)
* **Analytical Frameworks:** Root Cause Analysis, Risk Matrix Assessment, Customer Sentiment (CSAT) Evaluation

## Dashboard Architecture
The Power BI solution is divided into three strategic quadrants:

### 1. Executive Performance Summary
A high-level health check of the platform's revenue and operations.
* **Key Metrics:** Total Revenue, Order Volume, Average Order Value, Average Delivery Days, and MoM Revenue Growth.
* **Visuals:** Revenue vs. Volume Day-of-Week trending, Top Product categories, Payment Type distribution, and a Logistics Health pipeline.

### 2. Customer Sentiment & Root Cause Analysis
An operational deep dive isolating the drivers behind platform detractors.
* **Key Metrics:** Platform Defect Rate (1-3 Star Reviews) vs. 5-Star Reviews.
* **Visuals:** A customized Decomposition Tree tracing the exact correlation between product categories, delivery times, and defect rates, paired with a filtered Voice of the Customer table for qualitative feedback.

### 3. Geographic Performance & Vendor Risk
A vendor governance tool identifying high-risk partners.
* **Visuals:** A custom Brazilian State Shape Map using DAX `SWITCH` logic to map abbreviated seller states to full nomenclature for accurate geographic plotting.
* **The Risk Matrix:** A dynamic quality-control table sorting top-grossing sellers (using human-readable DAX aliases) by their Platform Defect Rate to identify high-revenue merchants causing brand damage.

## Data Governance & SQL Processing
Before ingestion into Power BI, the Kaggle dataset underwent rigorous auditing in MySQL to ensure reporting accuracy:
* **Integrity Checks:** Executed primary key and referential integrity checks across customer, order, and seller tables.
* **Data Cleansing:** Performed null audits and cast correct data types for precise date-time intelligence (e.g., delivery vs. estimated dates).
* *Note: The full suite of SQL cleaning queries is available in the `sql_queries/` folder of this repository.*

## Key Business Insights
1. **Logistics Drive Sentiment:** The Decomposition Tree reveals a severe, direct correlation between extended delivery days and elevated Platform Defect Rates, particularly in high-volume categories like Bed & Bath.
2. **Vendor Concentration Risk:** The Risk Matrix successfully isolates a subset of high-revenue sellers operating with defect rates exceeding 25%, flagging them for immediate quality control intervention.
3. **Geographic Cost Barriers:** Revenue is heavily concentrated in the Southeast (São Paulo, Rio de Janeiro), dictated by logistical feasibility and freight economics.

## How to Use This Repository
1. Clone the repository to your local machine.
2. Review the SQL scripts in the `sql_queries` folder to see the data transformation logic.
3. Open the `Olist_Ecommerce_Dashboard.pbix` file in Power BI Desktop to interact with the visualizations (requires Power BI Desktop to view the live cross-filtering).
