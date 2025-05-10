# Power BI Sales Analysis Project Overview

![image](https://github.com/user-attachments/assets/20224a80-d6d5-44d9-8ef4-d06f96cc4375)

## 1. Dataset & Dimensions (Figure 1)
- **Sales Categories**  
  Electronics, Footwear, Clothing, Home Appliances, Accessories, Kitchenware, Bags, Personal Care  
- **Tables**  
  - **Fact Table**: 3,510 order records with columns  
    - Date (dd/mm/yyyy)  
    - CustomerID  
    - PromotionID  
    - ProductID  
    - Units Sold  
  - **Dim Customers**  
  - **Dim Product**  
  - **Dim Promotion**

---

![image](https://github.com/user-attachments/assets/e86a5d47-f626-4716-9bce-62b4b11374b2)


## 2. Data Preparation (Figure 2)
In **Power Query Editor**, I added calculated columns to the Fact Table:
| Column              | Calculation                                                  |
|---------------------|--------------------------------------------------------------|
| Price Per Unit      | From `Dim Product[Price Per Unit]`                           |
| Total Sales         | `[Units Sold] * [Price Per Unit]`                            |
| Discount Percentage | `IF [Discount] > 0 THEN ROUND([Discount] / [Total Sales], 2)`|
| Discount            | `[Total Sales] - [Net Sales]`                                |
| Net Sales           | `[Total Sales] - [Discount]`                                 |
| Profit              | `[Net Sales] * Margin%` (e.g. 10%)                           |

---

![image](https://github.com/user-attachments/assets/232044c3-9043-471e-8db9-470129147d38)


## 3. Data Model (Figure 3)
Star schema with cardinalities:

- **Date Table 1**: default relationship for standard filtering  
- **Date Table 2**: secondary relationship via `USERELATIONSHIP` for comparing two periods

---

![image](https://github.com/user-attachments/assets/52ce099c-ac00-4cae-8c3b-6c1137b11e0f)


## 4. Project Requirements (Figure 4)
1. Top/Bottom 5 products by Sales / Profit / Quantity Sold  
2. Sales trends over time (daily, monthly, quarterly, annually)  
3. Relationship between Sales & Profit  
4. Compare Sales / Profit / Quantity between any two user-selected periods  
5. Average discount offered per promotion category  
6. Total number of orders  
7. Detail table of all remaining fields, filterable by Product, Date, Customer, Promotion  
8. Sales by different cities

---

![image](https://github.com/user-attachments/assets/d4cc4d22-be5e-4d28-8cbd-80223b0cf17c)


## 5. Core Visuals (Figure 5)
- **Scatter Plot**: Sales vs. Profit  
- **Bar Chart**: Average Discount by Promotion Category  
- **Card**: Total Number of Orders  
- **Map**: Sales by City

---

![image](https://github.com/user-attachments/assets/2bdefe38-86c1-484f-8840-faabb6010997)


## 6. Top/Bottom 5 Products (Figure 6)
- **Top 5** by Sales, Profit, Quantity  
- **Bottom 5** by Sales, Profit, Quantity  

---

![image](https://github.com/user-attachments/assets/5141012b-83db-493f-80b6-d85e88057242)


## 7. Two-Period Comparison (Figure 7)
User selects **Date Filter 1** and **Date Filter 2**, then compares:
- Total Sales  
- Total Profit  
- Total Quantity Sold  

**DAX Measures**  
```dax
Sum of Net Sales =
CALCULATE(
  SUM('Fact Table'[Net Sales]),
  ALL('Date Table 1'),
  USERELATIONSHIP('Date Table 2'[Date], 'Fact Table'[Date (dd/mm/yyyy)])
)

Total Profit =
CALCULATE(
  SUM('Fact Table'[Profit]),
  ALL('Date Table 1'),
  USERELATIONSHIP('Date Table 2'[Date], 'Fact Table'[Date (dd/mm/yyyy)])
)
```


## 8. Detailed Orders Table

The final visual is an interactive **table** that lets users drill into every individual order record. At the top are four **slicers** for filtering:

- **Date** (range or calendar picker)  
- **Customer Name**  
- **Product Name**  
- **Promotion Name**  

Below the slicers, the table displays all relevant fields for each matching order:

![image](https://github.com/user-attachments/assets/a6160cf4-0fab-4015-8b82-cdf7275db58a)


As users adjust any of the slicers, the table dynamically updates to show only the orders that meet those criteria, enabling detailed inspection of transaction-level data.  

