---
title: "Create a Pivot Table with View Entities"
url: /refguide/view-entity-pivot-table/

---

## Introduction

Use a view entity to create a pivot table. A pivot table is a table that contains a summary of another, more extensive table.

For this purpose of this use case, the following domain model is used:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/view-archived-data/domain-model.png" >}}

## Scenario

Suppose you are running a business and this is your shop's domain model. It includes:

* A *Product* entity that stores information of all the products sold
* An *Order* entity, which holds information about individual orders
* An *OrderLine* entity, which is a single item in an order

You want to analyze seasonal sales volumes, which is how much your business makes from sales in each quarter per year. You want your table to resemble the following:

| Year | TotalSales_Q1 | TotalSales_Q2 | TotalSales_Q3 | TotalSales_Q4| 
|------|---------------|---------------|---------------|---------------|
| 1996 | € …           | € …           |  € …          |  € …          |   
| 1997 | € …           | € …           |  € …          |  € …          | 
| 1998 | € …           | € …           |  € …          |  € …          | 
| ...  | …             | …             |  …            |  …            |

## Create a Pivot Table

1. Create a view entity that shows each order together with its total value, calculated from the *OrderLine* entity. Name this entity *OrderVE*.
2. Add the following query to the view entity:

```
SELECT
  o.OrderId as OrderId
  , CAST(DATEPART(QUARTER, o.OrderDate) as INTEGER) as OrderQuarter
  , CAST(DATEPART(YEAR, o.OrderDate) as INTEGER) as OrderYear
  , o.RequiredDate as RequiredDate
  , o.ShippedDate as ShippedDate
  , SUM(ol.UnitPrice * ol.Quantity * (1 - ol.Discount)) as TotalOrderValue
  , SUM(ol.Quantity) as TotalProductCount
  , COUNT(*) as UniqueProductCount
FROM Shop."Order" as o
  JOIN o/Shop.OrderLine_Order/Shop.OrderLine as ol
GROUP BY o.OrderId, o.OrderDate, o.RequiredDate, o.ShippedDate
```

{{% alert color="info" %}}

With view entities, you can take the relevant component of `DateTime` as a column using the `DATEPART` function.

{{% /alert %}}

3. Create another view entity named *OrderQuarterlyPivotVE*. This entity will show a table, similar to the format above.
4. Add the following query to this entity:

```
SELECT
    o.OrderYear as OrderYear,
    SUM(CASE WHEN o.OrderQuarter = 1 THEN o.TotalOrderValue ELSE 0 END) as TotalSales_Q1,
    SUM(CASE WHEN o.OrderQuarter = 2 THEN o.TotalOrderValue ELSE 0 END) as TotalSales_Q2,
    SUM(CASE WHEN o.OrderQuarter = 3 THEN o.TotalOrderValue ELSE 0 END) as TotalSales_Q3,
    SUM(CASE WHEN o.OrderQuarter = 4 THEN o.TotalOrderValue ELSE 0 END) as TotalSales_Q4
FROM Shop.OrderVE o
GROUP BY o.OrderYear
```

5. Run your app locally and use the Live Preview functionality to preview the data.
6. Click **OK** to save the data. 
7. Create a page that shows the pivot table by right-clicking the entity > **Generate overview pages**. 