---
title: "View Archived Data"
url: /refguide/view-archived-data/

---

## Introduction

View entities can be used to make a consolidated view of archived historical and active data, allowing access to both datasets in a single query, while keeping track of its origins. By consolidating, the active entity is smaller, enabling faster data retrieval and improving app performance. 

For this purpose of this use case, the following domain model is used:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/view-archived-data/domain-model.png" >}}

## Create a View Entity 

Create a view entity that contains the columns of the *ActiveProducts* and *DiscontinuedProducts* entities, as well as an additional column, *Archived*. The *Archived* column will indicate the origin table of each product. 

1. Open your domain model and add a new view entity named *AllProducts*. 
2. Add the following query to your entity:

```
SELECT
    ap.ProductId as ProductId,
    ap.ProductName as ProductName,
    ap.QuantityPerUnit as QuantityPerUnit,
    ap.Category as Category,
    ap.Price as Price,
    cast(FALSE as BOOLEAN) as Archived
FROM Shop.ActiveProducts ap
UNION
SELECT
    dp.ProductId as ProductId,
    dp.ProductName as ProductName,
    dp.QuantityPerUnit as QuantityPerUnit,
    dp.Category as Category,
    dp.Price as Price,
    cast(TRUE as BOOLEAN) as Archived
FROM Shop.DiscontinuedProducts dp
```

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/view-archived-data/all-products-ve.png" >}}

3. Run your app locally and view the new data grid. 

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/view-archived-data/data-grid.png" >}}