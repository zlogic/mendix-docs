---
title: "Decoupling APIs"
url: /refguide/decoupling-apis/

---

## Introduction

Exposing view entities instead of the underlying persistable entity takes away the complexity of the underlying schema. This helps you prevent frequent API changes in case the data model changes. It also allows you to consolidate data from multiple tabs in a single API.

For this purpose of this use case, the following domain model is used:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/decoupling-apis/domain-model.png" >}}

## Creating a View Entity

In this scenario, you want to make an API call that returns *Products*, and allows you to filter the results by Category. To do this, create a single view entity and expose it as an OData resource. 

1. Open your domain model and create a view entity called *ProductCategoryVE*.
2. Add the following query to the OQL editor:

```
SELECT
  p.ProductId as ProductId
  , p.ProductName as ProductName
  , p.QuantityPerUnit as QuantityPerUnit
  , p.Discontinued as Discontinued
  , c.CategoryName as Category
  , c.CategoryId as CategoryId
FROM Shop.Product as p
  JOIN p/Shop.Product_Category/Shop.Category as c
```

1. Right-click this entity and select **Publish in OData service**. Name this service *POS_ProductCategory*.
2. Add `ProductId` as a key attribute, then click **OK**.

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/decoupling-apis/key-attribute.png" >}}


5. In the Entity field, double-click the **ProductId** attribute. 
6. Uncheck the box **Can be empty**, then click **OK**. 
   
{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/decoupling-apis/can-be-empty.png" >}}

7. Run your app locally and test the functionality. 
