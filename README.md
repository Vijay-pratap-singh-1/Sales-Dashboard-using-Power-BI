# Sales Performance Dashboard — Power BI

**Project:** Interactive Sales Performance Dashboard  
**Author:** Vijay Pratap Singh  
**Created with:** Power BI Desktop

---

## 📌 Overview
This Power BI project is an interactive dashboard to monitor sales performance across KPIs, product categories, customer segments, and regions (North / South / East / West). It provides quick decision-ready insights via KPI cards, trend charts, and geographic mapping.

---

## 📊 Key Features
- KPI Cards: Total Sales, Total Profit, Total Orders, Total Quantity Sold  
- Visuals:
  - Bar chart — Sales by Product Category
  - Bar chart — Sales by Customer Segment
  - Line chart — Sales by Month & Year
  - Line chart — Profit by Month & Year
  - Filled Map — Sales by Region (US)
- Filter: Region slicer (North, South, East, West)
- Custom calendar table for time intelligence

---

## 🛠 Files in this repo
- `Sales_Dashboard.pbix` — Power BI report file (add to repo)  
- `README.md` — This file  
- `images/` — (optional) include dashboard screenshots: `dashboard_overview.png`

---

## 💾 Data model (tables)
- `Sales` — transactional sales data (order id, date, product, customer, sales, profit, quantity, region, segment, category)  
- `Sheet1` — product/customer reference data (if used)  
- `Calendar` — custom date table created in Power Query

**Recommended relationships**
- `Calendar[Date]` (1) → (many) `Sales[OrderDate]`  
- `Sales[ProductID]` → `Sheet1[ProductID]` (if product lookup exists)  
- `Sales[CustomerID]` → `Sheet1[CustomerID]` (if customer lookup exists)

---

## 🔧 Power Query — Calendar (M code)
Create a new blank query and paste this in the Advanced Editor:

```m
let
  StartDate = #date(2023, 1, 1),
  NumberOfDays = 731, // adjust as needed
  DateList = List.Dates(StartDate, NumberOfDays, #duration(1,0,0,0)),
  TableFromList = Table.FromList(DateList, Splitter.SplitByNothing(), {"Date"}),
  ChangedType = Table.TransformColumnTypes(TableFromList, {{"Date", type date}}),
  AddYear = Table.AddColumn(ChangedType, "Year", each Date.Year([Date]), Int64.Type),
  AddMonth = Table.AddColumn(AddYear, "MonthNumber", each Date.Month([Date]), Int64.Type),
  AddMonthName = Table.AddColumn(AddMonth, "MonthName", each Date.ToText([Date], "MMMM")),
  AddQuarter = Table.AddColumn(AddMonthName, "Quarter", each "Q" & Text.From(Date.QuarterOfYear([Date])))
in
  AddQuarter
