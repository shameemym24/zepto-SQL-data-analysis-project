
# 🛒 Zepto E-Commerce Inventory Data Analytics Project

## 🧭 Overview
This project simulates a **real-world e-commerce inventory analytics scenario** inspired by Zepto — one of India’s leading quick-commerce platforms.  
The objective was to work with **messy, unstructured inventory data**, perform cleaning, exploration, and derive **business insights** using SQL.

This end-to-end project demonstrates how data analysts in retail or e-commerce:
- Set up and manage **inventory databases**
- Perform **Exploratory Data Analysis (EDA)** to explore product categories, pricing inconsistencies, and stock availability
- Conduct **Data Cleaning** to handle missing or invalid entries and convert units (paise → rupees)
- Write **business-driven SQL queries** to analyze pricing, inventory, stock levels, and revenue trends

---

## 📂 Dataset

**File Name:** `zepto_v2.csv`  
**Records:** ~5,000 rows  
**Description:** Each record represents a unique SKU (Stock Keeping Unit) from Zepto’s product listings.

| Column | Description |
|---------|-------------|
| `sku_id` | Unique SKU identifier |
| `category` | Product category (e.g., Snacks, Beverages, Personal Care) |
| `name` | Product name |
| `mrp` | Maximum Retail Price (originally in paise) |
| `discountPercent` | Discount percentage applied |
| `availableQuantity` | Current stock available |
| `discountedSellingPrice` | Selling price after discount (in paise) |
| `weightInGms` | Product weight in grams |
| `outOfStock` | Boolean indicating stock availability |
| `quantity` | Number of units or packages |

---

## 🔍 Project Steps

### 1️⃣ Database Setup
Created the **`zepto`** table and defined data types to simulate an e-commerce inventory system.

```sql
CREATE TABLE zepto (
  sku_id SERIAL PRIMARY KEY,
  category VARCHAR(120),
  name VARCHAR(150) NOT NULL,
  mrp NUMERIC(8,2),
  discountPercent NUMERIC(5,2),
  availableQuantity INTEGER,
  discountedSellingPrice NUMERIC(8,2),
  weightInGms INTEGER,
  outOfStock BOOLEAN,
  quantity INTEGER
);
````

---

### 2️⃣ Data Exploration (EDA)

Explored and profiled the dataset to understand its structure, data types, and inconsistencies.

**Key Tasks:**

* Counted total rows and viewed sample records
* Checked for missing values across all columns
* Identified unique product categories
* Compared in-stock vs. out-of-stock products
* Found duplicate products appearing under multiple SKUs

```sql
SELECT COUNT(*) FROM zepto;
SELECT DISTINCT category FROM zepto ORDER BY category;
SELECT outOfStock, COUNT(*) FROM zepto GROUP BY outOfStock;
```

---

### 3️⃣ Data Cleaning

Cleaned and standardized the dataset to ensure accuracy and consistency.

**Actions Taken:**

* Removed invalid rows (MRP or Selling Price = 0)
* Converted **MRP and discountedSellingPrice** from **paise to rupees**
* Verified numeric consistency and logical ranges

```sql
DELETE FROM zepto WHERE mrp = 0;
UPDATE zepto
SET mrp = mrp / 100.0,
    discountedSellingPrice = discountedSellingPrice / 100.0;
```

✅ Result: Clean, structured dataset ready for business analysis.

---

### 4️⃣ Business Analysis (SQL Insights)

Developed and executed **business-driven SQL queries** to uncover insights related to pricing, stock, and revenue.

#### 🏷️ Top 10 Products with Highest Discounts

```sql
SELECT name, mrp, discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;
```

#### 💰 Category-Wise Estimated Revenue

```sql
SELECT category,
SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zepto
GROUP BY category
ORDER BY total_revenue DESC;
```

#### 📦 Out-of-Stock High MRP Products

```sql
SELECT name, mrp
FROM zepto
WHERE outOfStock = TRUE AND mrp > 300
ORDER BY mrp DESC;
```

#### 🧾 Top 5 Categories with Highest Average Discount

```sql
SELECT category,
ROUND(AVG(discountPercent), 2) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
```

#### ⚖️ Best Value Products by Price per Gram

```sql
SELECT name, weightInGms, discountedSellingPrice,
ROUND(discountedSellingPrice / weightInGms, 2) AS price_per_gram
FROM zepto
WHERE weightInGms >= 100
ORDER BY price_per_gram;
```

---



## 📈 Results & Insights

✅ Cleaned and standardized messy inventory data by removing invalid and null records.

✅ Converted pricing from **paise to rupees**, improving readability and consistency.

✅ Identified **high-value, out-of-stock products** for potential restocking.

✅ Calculated **category-wise revenue** and **discount trends** for better business decision-making.

✅ Designed SQL-based framework for ongoing **inventory performance monitoring**.

---

## ⚙️ How to Run

1️⃣ **Clone the repository**

```bash
git clone https://github.com/shameemym24/zepto-sql-analysis.git
cd zepto-sql-analysis
```

2️⃣ **Import the dataset**

```sql
\COPY zepto(category,name,mrp,discountPercent,availableQuantity,discountedSellingPrice,weightInGms,outOfStock,quantity)
FROM 'path/to/zepto_v2.csv' DELIMITER ',' CSV HEADER;
```

3️⃣ **Run SQL Scripts**
Execute `Zepto_SQL_data_analysis.sql` in your SQL editor (PostgreSQL/MySQL/SQL Server).

4️⃣ **(Optional)** Create a Power BI dashboard using the cleaned dataset.

---


## 👩‍💻 About the Author
Hey, I'm Shameem Yousuf -- Data Analyst skilled in SQL, Python, Power BI, and Excel with hands-on experience in data cleaning, visualization, and reporting.

📍 Dubai, UAE

📧 [shameem.yousuf47@gmail.com](mailto:shameem.yousuf47@gmail.com)

🔗 [LinkedIn](https://www.linkedin.com/in/shameem90)

---

⭐ *“Turning messy e-commerce data into meaningful business insights.”*








