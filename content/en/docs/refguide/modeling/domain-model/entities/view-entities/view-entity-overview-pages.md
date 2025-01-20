---
title: "Creating Overview Pages"
url: /refguide/view-entity-overview-pages/

---

## Introduction

Use view entities to show data across multiple associated entities.

For this purpose of this use case, the following domain model is used:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/domain-model.png" >}}

{{% alert color="info" %}}

You must have an existing data grid with data inputted into it prior to using a view entity. Do this by generating overview pages using the entities in the above domain model, then adding sample data into it. 

{{% /alert %}}

## Create a Data Grid 

In this scenario, create an overview page that lists each product in a [data grid]( /appstore/modules/data-grid-2/), including information about its category and supplier. 
With view entities, you do not have to manage associations when showing the data in a data grid. This means that all fields are filterable and sortable, which allows for higher performance and flexibility.

Create a view entity that combines only the relevant attributes of the entities *Product*, *Supplier*, and *Category*. To do this, follow these steps:

1. Open your domain model and add a new view entity.
2. Name the view entity *ProductOverviewVE*.
3. Add the following query to the OQL editor: 

```
SELECT
  p.ProductId as ProductId,
  p.ProductName as ProductName,
  p.QuantityPerUnit as QuantityPerUnit,
  p.Discontinued as Discontinued,
  s.CompanyName as Supplier, 
  c.CategoryName as Category
FROM Shop.Product as p
  JOIN p/Shop.Product_Supplier/Shop.Supplier as s
  JOIN p/Shop.Product_Category/Shop.Category as c

```

4. Click OK. The view entity is added to your domain model.

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/product-overview-ve.png" >}}

5. Generate an overview page by right-clicking the view entity > **Generate overview pages**.
6. Run your app locally, then click **View App**. You should see the data grid populated with the information that was previously added.

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/ live-data-grid.png" >}}

{{% alert color="info" %}}

In the new data grid created by  *ProductOverviewVE*, the **Edit** button is not functional. You can either hide or remote this button, or follow the steps below to [update the product microflow](#update-microflow) or [add a new product](#add-product) to the grid.

{{% /alert %}}

## Alternative to Calculated Attributes

You can use view entities to search and sort items, as well as receive the calculated attribute (or Total Value). This is only calculated once, unlike a non-persistable entity, where it is calculated each time the object is accessed. 

For example, the *OrderLine* entity represents a single entry in an order. It has the following attributes:

* `UnitPrice`
* `Quantity`
* `Discount`

### Get Total Value

Suppose you want to get the total value of each order line, which is given by the formula `Total = UnitPrice * Quantity* (1 – Discount)`. To do this, follow these steps:

1. Create a view entity and name it *OrderLineWithTotalVE*.
2. Add the following query to the OQL editor:

```
SELECT
  ol.OrderLineId as OrderLineId,
  ol.Quantity as Quantity,
  ol.UnitPrice as UnitPrice,
  ol.Discount as Discount,
  (ol.Quantity * ol.UnitPrice * (1 - ol.Discount)) as Total
FROM Shop.OrderLine ol
```

3. Generate an overview page by right-clicking the view entity > Generate overview pages.
4. Run your app locally, then click **View App**. Use this view entity in a data grid to show the total value.

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/total-value.png" >}}

### Join OrderLine and Order Entities

Calculate the total value of an order by joining the `OrderLine` and `Order`tables. To do this, follow the steps below:

1. Create a new view entity and name it *OrderWithTotalValueVE*.
2. Add the following query to the OQL editor: 

```
SELECT
  o.OrderId as OrderId,
  o.OrderDate as OrderDate,
  o.RequiredDate as RequiredDate,
  o.ShippedDate as ShippedDate,
  SUM(ol.UnitPrice * ol.Quantity * (1 - ol.Discount)) as TotalOrderValue,
  SUM(ol.Quantity) as TotalProductCount,
  COUNT(*) as UniqueProductCount
FROM Shop."Order" as o
  JOIN o/Shop.OrderLine_Order/Shop.OrderLine as ol
GROUP BY o.OrderId, o.OrderDate, o.RequiredDate, o.ShippedDate
```

3. Generate an overview page by right-clicking the view entity > Generate overview pages.
4. Run your app locally, then click **View App**. This results in a view entity that shows the total value of every order.

{{% alert color="info" %}} 
Notice the quotation marks in `Shop.”Order”`. This is because `Order` is a reserved keyword in OQL. To avoid ambiguity, quotation marks are put around the word. 
{{% /alert %}}

## Update Underlying Persistent Entities

On the product overview page above, there is no button to add or modify a product. This can be added to a view entity to update its corresponding persistable entity object.

1. Create a microflow and name it *ProductOverview*. This microflow takes the `ProductOverviewVE` object.
   
{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/product-overview-microflow.png" >}}

2. Add a [retrieve]( /refguide/retrieve/) activity. In this activity, retrieve a `Product`object from the database. Configure the activity with the following details: 

  * Use the following XPath constraint: 
`[(ProductId = $ProductOverviewVE/ProductId)]`
  * In the Options field, set Range to **First** 
  
3. Add a [Change Object]( /refguide/change-object/) activity. Configure the activity with the following details: 

* Change the attributes of `Product` to reflect those of `ProductOverviewVE` 
* In the Options field, set Range to **First** 
* In the Commit field, select **Yes**
  
3. Open the NewEdit page that was generated by ProductOverviewVE.
4. Place a data view with `ProductOverviewVE`as the data source. 
5. Remove the Supplier and Category fields.
6. Add a new custom column to the data grid on the Overview page and add an **Edit** button.
7. Link this button to the Edit Product page. 

Once these steps are complete, run your app locally and test the functionality.

## Update Associated Product Category and Supplier

Add the capability to update a product’s associated category and supplier. To do this, follow these steps:

1. Add the columns `CategoryId` and `SupplierId` to `ProductOverviewVE`. Use the following query:

```
SELECT
  p.ProductId as ProductId,
  p.ProductName as ProductName,
  p.QuantityPerUnit as QuantityPerUnit,
  p.Discontinued as Discontinued,
  s.CompanyName as Supplier,
  c.CategoryName as Category,
  s.SupplierId as SupplierId,
  c.CategoryId as CategoryId
FROM Shop.Product as p
  JOIN p/Shop.Product_Supplier/Shop.Supplier as s
  JOIN p/Shop.Product_Category/Shop.Category as c
```

2. Create two more view entities that retrieve ID and Names from the Supplier and Category tables. Use the following two queries:

```
SELECT
  s.SupplierId as SupplierId,
  s.CompanyName as SupplierName
FROM Shop.Supplier s
```

```
SELECT
  c.CategoryId as CategoryId,
  c.CategoryName as CategoryName
FROM Shop.Category c
```

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/supplier-and-category-ve.png" >}}

3. On the Edit ProductOverviewVE page, add a Combo box inside the data view.
4. In the Data source field, select **Database**, then under Selectable objects, click **Edit** > **Entity (path)** > **CategoryNames**. Then, click **Select**.  
5. In the Attribute field, select `CategoryId` for the Value and Attribute. 
6. In the Caption field, next to Caption, click **Select** > **CategoryName**.

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/combo-box.png" >}}

7. Add another Combo box and repeat the above steps for the Supplier view entity. 

Your final form should look like this:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/product-overview-page.png" >}}

## Update Product Microflow {#update-microflow}

Once the Product page is complete, you must update the related microflow. 

1. Open your microflow and add a Retrieve activity after the previous Retrieve product acidity. 
2. Retrieve a Category object from the database where `CategoryId = $ProductOverviewVE/CategoryId`.
3. Add another Retrieve activity for the supplier. 
4. In the existing Change Product activity, add the Category and Supplier associations and set them to their corresponding objects. 

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/update-product-microflow.png" >}}

When you run your app, you should now be able to update the product’s category and supplier. 

## Add a New Product {#add-product}

You can use a view entity to add a new product into the existing database. Follow the steps below:

1. Create a new microflow and name it *CreateProduct*
2. Add a Create object activity to create a *Product* (the persistable entity) object. Leave all the attributes blank.
3. Set Commit to **Yes**.
4. Place a Retrieve object activity right after the previous activity. 
5. Retrieve a `ProductOverviewVE` that corresponds to the new `Product` object ( `ProductId = $NewProduct/ProductId`). 
6. Add a Show page activity and set it to open the Edit Product page. Use `NewProductVE` as the page parameter. 
7. Add a button to the Product Overview page and link to the Create Product microflow. 

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-product-microflow.png" >}}