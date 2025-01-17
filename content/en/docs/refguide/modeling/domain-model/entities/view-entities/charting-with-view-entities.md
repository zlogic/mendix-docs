---
title: "Charting with View Entities"
url: /refguide/charting-with-view-entities/

---

## Introduction

Use view entities to create charts in Studio Pro using aggredated data. 

For this purpose of this use case, the following domain model is used:

## Create Charts with View Entities

In this scenario, imagine you own a small business. To keep track of what is being sold, you want to visualize your sales. You want to  see how much you make in sales each year, and how each product category contributes to the number. You would like your chart to look similar to the one below:

### Create a View Entity

Mendix charts do not take associations, so you usually have to take extra steps to use a non-persistable entity as a source. Using view entities is a faster and simpler way to create a chart.

For the chart, you need the year on the X axis, and the total sales on the Y axis. Create a view entity that groups this data by product category. To do this, follow the steps below:

1. Open your domain model and create a new view entity named *YearlySalesbyCategoryVE*. 
2. Add the following query to the view entity:

```
SELECT
  CAST(DATEPART(YEAR, o.OrderDate) as INTEGER) as OrderYear
  , c.CategoryId as CategoryId
  , c.CategoryName as CategoryName
  , SUM(ol.Quantity * ol.UnitPrice * (1 - ol.Discount)) as TotalSales
FROM SalesDashboard."Order" as o
  JOIN SalesDashboard.OrderLine_Order/SalesDashboard.OrderLine as ol
  LEFT JOIN SalesDashboard.OrderLine_Product/SalesDashboard.Product as p
  LEFT JOIN SalesDashboard.Product_Category/SalesDashboard.Category as c
GROUP BY c.CategoryId, c.CategoryName, CAST(DATEPART(YEAR, o.OrderDate) as INTEGER)
``` 

View entities allow you to calculate and aggregate the total sales in a single line, You can also take the year by using `DateTime`. 

### Create the Chart

Use the new view entity to create a chart.

1. Create a new page (or use any existing page) and from the Toolbox, add a Column chart to the page.
2. Double-click the chart and in the Data Source field, click **New**. 
3. Edit the chart by filling out the following:

* Data set - **Multiple series**
* Data source - **YearlySalesByCategoryVE**
* Group by - **YearlySalesByCategoryVE**
* X axis - **Order Year**
* Y axis - **TotalSales**

4. Click **OK** to save. The outcome should look like this: 
5. Run your app locally and you should see the chart populated with your data. 
