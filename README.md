# 📦 Optimizing Procurement & Inventory Health | Supply Chain Management | Power BI
<img width="1689" height="933" alt="image" src="https://github.com/user-attachments/assets/1a837658-7a57-4be9-b830-fe89f39a9e8d" />

**Author:** Huong Le | **Date:** May 2025 | **Tools Used:** BI

## Table of Contents
1. [📌 Background & Overview](#background--overview)
2. [📂 Dataset Description & Data Structure](#dataset-description--data-structure)
3. [🧠 Design Thinking Process](#design-thinking-process)
4. [📊 Key Insights & Visualizations](#key-insights--visualizations)
5. [🔎 Final Conclusion & Recommendation](#final-conclusion--recommendation)

---

## 📌 Background & Overview

**Objective:**
To reduce operational risk by optimizing vendor reliability, balancing inventory levels, and analyzing procurement spend efficiency.

### 📖 What is this project about?
This project focuses on designing a Power BI analytics dashboard using the **AdventureWorks** dataset, utilizing data from Purchasing, Inventory, Vendors, and Product tables.

The goal is to equip the supply chain and procurement teams with clear, actionable insights to:
*   Analyze **$64M** in total purchase spend and identify cost variance trends.
*   Monitor vendor performance to distinguish between high-reliability partners and risk-prone suppliers.
*   Visualize inventory health to solve the "Stockout vs. Overstock" imbalance.
*   Enable data-driven decisions for reorder points and vendor contract negotiations.

### 👤 Who is this project for?
*   **Procurement Managers** needing to evaluate vendor compliance and negotiate better terms.
*   **Supply Chain Directors** responsible for minimizing backorders and optimizing cash flow.
*   **Inventory Analysts** looking to identify slow-moving stock and adjust safety stock levels.
*   **Operations Leads** requiring real-time visibility into fulfillment rates and lead times.

## 📂 Dataset Description & Data Structure

### **📌 Data Source** 
- **Source**: Kaggle - Dataset of AdventureWorks
- **Size**: The **Purchase_OrderDetails** table contains **8,845** records.  
- **Format**: Pbix

### 📊 **Data Structure & Relationships**  

#### 1️⃣ **Tables Used:**  
The dataset consists of **7 main tables** used to build the purchasing dashboard:

- 📦 **Fact_Purchasing_OrderDetail** – Line-level order details.

<details>
<summary><strong>Table 1: Fact_Purchasing_OrderDetail</strong></summary>

| Column Name             | Description                                  |
|-------------------------|----------------------------------------------|
| `OrderQty`              | Quantity ordered                             |
| `ReceivedQty`           | Quantity received                            |
| `RejectedQty`           | Quantity rejected                            |
| `StockedQty`            | Quantity stocked                             |
| `LeadTime_Days`         | Lead time in days                            |
| `DelayDays`             | Number of delay days                         |
| `DueDate`, `OnTime`     | Delivery due date and on-time flag           |
| `IsBackorderedFlag`     | Indicates if the item was backordered        |
| `UnitPrice`             | Unit price of the item                       |
| `PurchaseOrderDetailID` | Line item identifier                         |
| `PurchaseOrderID`, `ProductID` | Foreign keys to orders and products     |

</details>

- 🏷️ **Fact_Product_Inventory** – Current inventory levels.

<details>
<summary><strong>Table 2: Fact_Product_Inventory</strong></summary>

| Column Name           | Description                                |
|------------------------|--------------------------------------------|
| `Quantity`             | Current inventory quantity                 |
| `Below Reorder Flag`   | Indicates if inventory is below reorder level |
| `BelowSafetyStock`     | Indicates stock is below safety threshold  |
| `OutOfStockProducts`   | Out of stock status                        |
| `ProductID`            | Foreign key to product                     |

</details>

- 🧾 **Dim_Product_Product** – Product master data.

<details>
<summary><strong>Table 3: Dim_Product_Product</strong></summary>

| Column Name           | Description                                |
|------------------------|--------------------------------------------|
| `ProductID`            | Unique product identifier                  |
| `Name`, `Class`, `Style` | Product characteristics                   |
| `SafetyStockLevel`     | Safety stock value                         |
| `ReorderPoint`         | Reorder threshold                          |
| `ListPrice`, `StandardCost` | Price and cost info                  |
| `ProductSubcategoryID` | Foreign key to product taxonomy            |

</details>

- 📄 **Dim_Purchasing_OrderHeader** – Order-level metadata.

<details>
<summary><strong>Table 4: Dim_Purchasing_OrderHeader</strong></summary>

| Column Name         | Description                                 |
|----------------------|---------------------------------------------|
| `PurchaseOrderID`    | Header-level order ID                       |
| `OrderDate`, `ShipDate` | Order creation and shipping date         |
| `VendorID`           | Foreign key to vendor                       |
| `TotalDue`, `Freight`, `SubTotal` | Order-level cost details      |

</details>

- 🧑‍💼 **Dim_Purchasing_Vendor** – Vendor master data.

<details>
<summary><strong>Table 5: Dim_Purchasing_Vendor</strong></summary>

| Column Name             | Description                            |
|--------------------------|----------------------------------------|
| `VendorID`               | Unique vendor ID                       |
| `Name`, `AccountNumber`  | Vendor info                            |
| `PreferredVendorLabel`   | Whether vendor is preferred            |
| `CreditRating`           | Vendor's credit rating                 |

</details>

- 🔗 **Dim_Purchasing_ProductVendor** – Product-vendor mapping.

<details>
<summary><strong>Table 6: Dim_Purchasing_ProductVendor</strong></summary>

| Column Name           | Description                              |
|------------------------|------------------------------------------|
| `ProductID`            | Linked product                           |
| `VendorID`             | Linked vendor                            |
| `MinOrderQty`, `MaxOrderQty` | Order quantity boundaries        |
| `StandardPrice`        | Standard unit cost                       |
| `AverageLeadTime`      | Vendor delivery time in days             |

</details>

- 🧱 **Dim_Product_ProductTaxonomy** – Product categories.

<details>
<summary><strong>Table 7: Dim_Product_ProductTaxonomy</strong></summary>

| Column Name           | Description                                |
|------------------------|--------------------------------------------|
| `ProductID`            | Product reference                          |
| `Category`, `Subcategory` | Product hierarchy                      |

</details>

- 🗂️ **Dim_Product_ProductTable** – Product category and hierarchy.

<details>
<summary><strong>Table 8: Dim_Product_ProductTable</strong></summary>

| Column Name             | Description                         |
|--------------------------|-------------------------------------|
| `ProductID`              | Linked product                      |
| `ProductCategoryID`      | Category reference                  |
| `ProductSubcategoryID`   | Subcategory reference               |
| `Category`               | Product category name               |
| `Subcategory`            | Product subcategory name            |

</details>

#### 2️⃣ Data Relationships:

<img width="1777" height="1039" alt="image" src="https://github.com/user-attachments/assets/9bb1c6c9-1065-46ea-a0a0-8a939dd45cc6" />

| **From Table**                  | **To Table**                     | **Join Key**                | **Relationship Type**                                      |
|--------------------------------|----------------------------------|-----------------------------|------------------------------------------------------------|
| `Fact_Purchasing_OrderDetail`  | `Dim_Purchasing_OrderHeader`     | `PurchaseOrderID`           | Many-to-One (many order lines per order header)            |
| `Fact_Purchasing_OrderDetail`  | `Dim_Product_Product`            | `ProductID`                 | Many-to-One (many order lines for one product)             |
| `Fact_Purchasing_OrderDetail`  | `Dim_Order_Date`                 | `DueDate` / `ModifiedDate`  | Many-to-One (orders map to one date)                       |
| `Dim_Purchasing_OrderHeader`   | `Dim_Purchasing_Vendor`          | `VendorID`                  | Many-to-One (multiple orders per vendor)                   |
| `Dim_Purchasing_ProductVendor` | `Dim_Purchasing_Vendor`          | `VendorID`                  | Many-to-One (vendor supplies many products)                |
| `Dim_Purchasing_ProductVendor` | `Dim_Product_Product`            | `ProductID`                 | Many-to-One (vendor offers multiple products)              |
| `Dim_Product_Product`          | `Dim_Product_ProductTaxonomy`    | `ProductSubcategoryID`      | Many-to-One (each product belongs to one subcategory)      |
| `Fact_Product_Inventory`       | `Dim_Product_Product`            | `ProductID`                 | Many-to-One (each inventory record linked to a product)    |

## 🧠 Design Thinking Process

<details>
<summary><strong>1️⃣ Empathize</strong></summary>

<img width="1656" height="906" alt="image" src="https://github.com/user-attachments/assets/9cd16584-5381-4f67-a11e-832d9b0ed0ad" />
<img width="1669" height="903" alt="image" src="https://github.com/user-attachments/assets/2717b1ed-704d-433c-b1be-4ed3911f43aa" />
<img width="1668" height="904" alt="image" src="https://github.com/user-attachments/assets/7829aab8-8a4b-4169-bb47-917ab4cf403f" />
</details>

<details>
<summary><strong>2️⃣ Define point of view</strong></summary>

<img width="1631" height="865" alt="image" src="https://github.com/user-attachments/assets/78220999-7415-40d6-9502-59610ba0fb5a" />
</details>

<details>
<summary><strong>3️⃣ Ideate</strong></summary>

<img width="1660" height="867" alt="image" src="https://github.com/user-attachments/assets/810f6170-c476-48b8-a474-1cec5278aff6" />
</details>

### 4️⃣ Prototype and review

This part is in the dashboard

## 📊 Key Insights & Visualizations

### 🔍 Dashboard Preview

### 📋 I. Overview

<img width="1752" height="977" alt="image" src="https://github.com/user-attachments/assets/fd9f6ff6-1693-44e7-b7ab-d0c47bbed1fd" />

<details>
<summary> <strong>⭐ Key Findings — Overview</strong></summary>

#### 🚨 1. Critical Data Anomaly: Q4 Purchasing Stop
*   **Observation:** The "Monthly Cumulative Purchase Spend" chart shows the Current Year (Blue line) climbing steadily to **$64M** by September, then **completely flatlining** for the rest of the year.
*   **Implication:** Unlike the previous year (Green area) which showed continued growth, this indicates either a total freeze on purchasing in Q4 or **missing data** for October, November, and December in the ERP system.

#### ⚠️ 2. Data Quality Alert: $22M Unclassified Spend
*   **Observation:** In the "Purchase Spend by Product Subcategory" chart, the largest bar is **"(Blank)"**, accounting for **$22M** (approx. 34% of total spend).
*   **Implication:** Over one-third of procurement costs are not mapped to a specific product category. This requires immediate data remediation to accurately track category performance.

#### 📉 3. Operational Health: Persistent Backorders
*   **Observation:** The overall **Backorder Rate is 9.42%**. The monthly trend shows this is not a seasonal spike but a consistent issue, fluctuating between **8.5% and 10.5%** year-round.
*   **Implication:** Nearly 1 in 10 orders faces a stock shortage, suggesting a systemic issue with inventory planning calculations or vendor fulfillment capabilities.

#### 💰 4. Vendor Concentration Risk
*   **Observation:** **Superior Bicycles** is the dominant vendor with **$4.6M** in spend, significantly higher than the next largest vendors (Professional Athletic Consultants at $3.1M).
*   **Implication:** The supply chain is heavily reliant on a single partner. Performance issues with Superior Bicycles (high backorder rates noted in other views) will disproportionately impact the entire company.

#### ✅ 5. The Fulfillment vs. Availability Paradox
*   **Observation:** The dashboard reports a **99.92% On-Time Fulfillment Rate** despite the **9.42% Backorder Rate**.
*   **Implication:** This indicates a potential conflict in KPI definitions. Vendors may be shipping "what they have" on time (high fulfillment score) but failing to ship the "complete order" (high backorder score).

> **💡 Managerial Takeaway:** While total spend is substantial ($64M), the "Blank" data category obscures $22M of insights. Immediate focus must be placed on classifying this data and investigating why purchasing data stops in Q4.
</details>

### 📈 II. Product Analysis

<img width="1741" height="980" alt="image" src="https://github.com/user-attachments/assets/8bfcc548-6698-4d5a-b168-182dbf5d039e" />

<details>
<summary> <strong>⭐ Key Findings — Overview</strong></summary>
  
#### ⚠️ 1. Portfolio Health Risk
*   **Observation:** **373 out of 504 products (74%)** are classified as "At-Risk."
*   **Implication:** The majority of the product catalog requires intervention, indicating a systemic failure in automated reordering logic.

#### 📉 2. The "High-Value" Stockout
*   **Observation:** **HL Mountain Frames** are the #1 product by spend ($1.13M) but also the #1 product by **Stock Shortfall (145 units)**.
*   **Implication:** The company is stocking out of its best-sellers/highest investment items, directly damaging revenue and ROI.

#### 🛑 3. Capital Misalignment (The "Long Tail" Problem)
*   **Observation:** While critical components are backordered (15.79% rate), the "Inventory Status" table flags low-value items (Ball Bearings, Caps, Bike Stands) as severely **Overstocked**.
*   **Implication:** Cash flow is tied up in slow-moving, low-value inventory. The dashboard explicitly recommends **"Pause Purchasing"** for these items.

> **💡 Managerial Takeaway:** Stop buying "Bike Stands" and "Bearings" immediately. Redirect that capital to solve the shortage of "HL Mountain Frames."
</details>

### III. 🤝 Vendor Performance

<img width="1735" height="972" alt="image" src="https://github.com/user-attachments/assets/5777218b-d3e5-47a4-a640-d759062fd2e4" />

<details>
<summary> <strong>⭐ Key Findings — Overview</strong></summary>

#### 🚨 1. The "High-Spend" Reliability Crisis
*   **Observation:** The top vendors by spend are the worst performers. **First Rate Bicycles** ($2.1M) and **Chicago City Saddles** ($3.0M) have backorder rates exceeding **21%**.
*   **Implication:** We are heavily investing in partners who fail to deliver 1 in 5 times. Contract renegotiations should prioritize "On-Time In-Full" (OTIF) penalties over price breaks.

#### 🛡️ 2. Strong Financial Vetting
*   **Observation:** The "Spend vs Credit Rating" scatter plot confirms that high-spend vendors (large bubbles) consistently have top-tier Credit Ratings (1).
*   **Implication:** Financial risk is well-managed. The supply chain is stable financially but unstable operationally.

#### ⚠️ 3. The "Static Lead Time" Blind Spot
*   **Observation:** The detail table shows an identical **Lead Time of 9 days** for almost every major vendor.
*   **Implication:** This indicates a data quality issue where "Standard Lead Time" is being used instead of "Actual Lead Time." Without accurate lead time data, safety stock calculations will fail, leading to the stockouts observed in the Product Analysis.

> **💡 Managerial Takeaway:** Initiate a Business Review with **Superior Bicycles** and **Chicago City Saddles**. Demand a reduction in Backorder Rate from ~20% to <5% as a condition for retaining the $7.6M combined spend.
</details>

## 🚀 Final Conclusion & Recommendation 

| Aspect | Insight | Recommendation |
| :--- | :--- | :--- |
| **Vendor Strategic Alignment** | - There is a **critical misalignment** between spend and reliability. Top vendors like **Superior Bicycles** and **Chicago City Saddles** absorb high capital ($7.6M combined) but deliver the worst performance, with **Backorder Rates >20%**. | - Initiate immediate **Quarterly Business Reviews (QBRs)** with top 5 vendors.<br>- Implement **performance penalties** for backorder rates exceeding 5% and prioritize alternative suppliers for high-risk SKUs. |
| **Inventory Health & Rebalancing** | - A **capital imbalance** exists: High-value, high-demand items (e.g., **HL Mountain Frames**) face critical stockouts (145 unit shortfall), while low-value Accessories remain **severely overstocked**. | - Execute an immediate **"Purchase Pause"** on all SKUs flagged as "Overstocked" (Accessories) to free up cash flow.<br>- Redirect this capital to urgently replenish critical **Components** and **Finished Goods**. |
| **Data Quality & Spend Visibility** | - **$22M in procurement spend** (34% of total) is currently unclassified **(Blank)**, and purchasing data **flatlines in Q4**, obscuring year-end performance visibility. | - Launch a **Data Remediation Project** to classify the $22M "Blank" spend within 30 days.<br>- Investigate the Q4 data gap to ensure the ERP system is correctly capturing year-end transactions. |
| **Operational Planning Parameters** | - **74% of the product portfolio** is "At-Risk." Additionally, the system shows a static **9-Day Lead Time** for almost all vendors, suggesting that **Safety Stock calculations** are based on default/incorrect data. | - Move from static to **dynamic lead times**.<br>- Update the ERP with **actual historical lead times** per vendor to allow the system to calculate accurate Safety Stock levels and Reorder Points. |

> **💡 Executive Summary:** The company is currently "funding its own failure" by overspending on unreliable vendors and overstocking low-value items. By fixing the data quality issues and shifting capital to reliable partners for high-demand goods, we can reduce the backorder rate from **9.42% to <5%** without increasing the total budget.
