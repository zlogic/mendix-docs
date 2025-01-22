---
title: "Creating Overview Pages"
url: /refguide/view-entity-overview-pages/

---

## Introduction

Use view entities to show data across multiple associated entities.

For this purpose of this use case, the following domain model is used:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/domain-model.png" >}}

{{% alert color="info" %}}

You must have an existing database containing data prior to using a view entity. Do this by generating overview pages using the entities in the above domain model, then adding sample data. 

{{% /alert %}}

## Create a Data Grid 

In this scenario, you will create an overview page that lists each product in a [data grid]( /appstore/modules/data-grid-2/), including information about its category and supplier. 
With view entities, you do not have to manage associations when showing the data in a data grid. This means that all fields are filterable and sortable, which allows for higher performance and flexibility.

Create a view entity that combines only the relevant attributes of the entities *Product*, *Supplier*, and *Category*. To do this, follow these steps:

1. Open your domain model and add a new view entity.
2. Name the view entity *ProductOverviewVE*.
3. Add the following query to the OQL editor: 

    ```sql
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
   
6. Add the new overview page to the navigation.

7. Run your app locally, then click **View App**. You should see the data grid populated with the information that was previously added.

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/ live-data-grid.png" >}}

{{% alert color="info" %}}

In the new data grid created by *ProductOverviewVE*, the **Edit** buttons is not functional. You can hide or remote this button, or follow the steps below to [update the product microflow](#update-microflow) or [add a new product](#add-product) to the grid.

{{% /alert %}}

## Alternative to Calculated Attributes

You can use view entities to search and sort items, as well as hold the calculated attribute (or Total Value). This is only calculated once, unlike a non-persistable entity, where it is calculated each time the object is accessed. 

For example, the *OrderLine* entity represents a single entry in an order. It has the following attributes:

* `UnitPrice`
* `Quantity`
* `Discount`

### Get Total Value

Suppose you want to get the total value of each order line, which is given by the formula `Total = UnitPrice * Quantity* (1 – Discount)`. To do this, follow these steps:

1. Create a view entity and name it *OrderLineWithTotalVE*.
2. Add the following query to the OQL editor:

    ```sql
    SELECT
      ol.OrderLineId as OrderLineId,
      ol.Quantity as Quantity,
      ol.UnitPrice as UnitPrice,
      ol.Discount as Discount,
      (ol.Quantity * ol.UnitPrice * (1 - ol.Discount)) as Total
    FROM Shop.OrderLine ol
    ```

3. Generate an overview page by right-clicking the view entity > **Generate overview pages**.
4. Add the new overview page to the navigation.
5. Run your app locally, then click **View App**. Use this view entity in a data grid to show the total value.

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/total-value.png" >}}

### Join OrderLine and Order Entities

You can calculate the total value of an order by joining the `OrderLine` and `Order`tables. To do this, follow the steps below:

1. Create a new view entity and name it *OrderWithTotalValueVE*.
2. Add the following query to the OQL editor: 

    ```sql
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
 
This results in a view entity that shows the total value of every order.  

{{% alert color="info" %}} 
Notice the quotation marks in `Shop.”Order”`. This is because `Order` is a reserved keyword in OQL. To avoid ambiguity, quotation marks are put around the word. 
{{% /alert %}}

3. Generate an overview page by right-clicking the view entity > **Generate overview pages**.
4. Add the new overview page to the navigation.
5. Run your app locally, then click **View App**. This results in a view entity that shows the total value of every order.

{{% alert color="info" %}} 
Notice the quotation marks in `Shop.”Order”`. This is because `Order` is a reserved keyword in OQL. To avoid ambiguity, quotation marks are put around the word. 
{{% /alert %}}

## Update Underlying Persistent Entities

On the Product overview page above, there is no button to add or modify a product. This can be added to a view entity to update its corresponding persistable entity object.

1. Create a microflow and name it *ACT_UpdateProduct*. 
2. Add a parameter and in the entity field, select *ProductOverviewVE*.
3. Add a [retrieve](/refguide/retrieve/) activity. In this activity, retrieve a *Product* object from the database. Configure the activity with the following details: 

    * Use the following XPath constraint:

        ```
        [(ProductId = $ProductOverviewVE/ProductId)]
        ```

    * In the Options field, set Range to **First** 
  
4. Add a [Change Object]( /refguide/change-object/) activity. Configure the activity with the following details: 

    * Add the attributes of `Product` to reflect those of `ProductOverviewVE` 
    * In the Options field, set Range to **First** 
    * In the Commit field, select **Yes**

    {{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/product-overview-microflow.png" >}}
  
5. Open the ProductOverviewVE_NewEdit page that was generated by *ProductOverviewVE*.
6. Remove the Product ID, Supplier, and Category fields.
7. Double-click the **Save** button and in the On click field, select **Call a microflow**.
8. Under Microflow, click **Select** > A**CT_UpdateProduct**. 
9. Click **OK**.
10. Open the ProductOverviewVE_Overview page.
11. Add a new custom column on the far right of the data grid.
12. Add a button to this new column.
13. Link this button to the ProductOverviewVE_NewEdit page. 

Once these steps are complete, run your app locally and test the functionality.

## Update Associated Product Category and Supplier

Add the capability to update a product’s associated category and supplier. To do this, follow these steps:

1. Open your domain model and double-click **ProductOverviewVE**.
2. Add `CategoryId` and `SupplierId` by adding the following query in the OQL editor:

    ```sql
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

3. Create two more view entities that retrieve ID and names from the Supplier and Category tables. Name these view entities *SupplierNamesVE* and *CategoryNamesVE*.
4. Add the following queries into the OQL editor. Add one to each entity:

    ```sql
    SELECT
      s.SupplierId as SupplierId,
      s.CompanyName as SupplierName
    FROM Shop.Supplier s
    ```

    ```sql
    SELECT
      c.CategoryId as CategoryId,
      c.CategoryName as CategoryName
    FROM Shop.Category c
    ```

    {{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-overview-pages/supplier-and-category-ve.png" >}}

5. On the ProductOverviewVE_NewEdit page, add a Combo box inside the data view. Include the following information:

    * In the Data source field, select **Database**
    * Under Selectable objects, click **Edit** > **Entity (path)** > **CategoryNamesVE** > **Select**
    * Under Target attribute, select **Category**
    * Under Caption, select **CategoryNames**

    {{< figure src="/attachments/refguide/modeling/domain-model/view-entities/combo-box.png" >}}

6. Add another Combo box and repeat the above steps for *SupplierNameVE* entity. 

The final data view should look like this:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/product-overview-page.png" >}}

## Update Product Microflow {#update-microflow}

Once the Product page is complete, update the related microflow. To do this, follow the steps below: 

1. Open the ACT_UpdateProduct microflow.
2. Add a Retrieve activity after the existing Retrieve product activity. 
3. Retrieve a Category object from the database. Configure the activity using the following details:
   
    * Use the following XPath constraint: 
    
        ```
        [CategoryId = $ProductOverviewVE/CategoryId]
        ```

    * In the Range field, select **First** 

4. Add another Retrieve activity to retrieve the Supplier object. Configure the activity using the following details: 

    * Use the following XPath constraint: 

        ```
        [SupplierId = $ProductOverviewVE/SupplierId]
        ```

    * In the Range field, select **First** 
  
5. In the existing Change Product activity, click **New** and add the Category and Supplier associations and set them to their corresponding objects. 

    {{< figure src="/attachments/refguide/modeling/domain-model/view-entities/update-product-microflow.png" >}}

When you run your app, you should now be able to update the product’s category and supplier. 

## Add a New Product {#add-product}

You can use a view entity to add a new product into the existing database. Follow the steps below:

1. Create a new microflow and name it *ACT_CreateProduct*
2. Add a Create object activity to create a *Product* (the persistable entity) object. Leave all the attributes blank.
3. Set Commit to **Yes**.
4. Place another Retrieve object activity after the previous activity. 
5. Retrieve `ProductOverviewVE` that corresponds to the new `Product` object. Configure it with the following details:

    * Use the following XPath constraint: 
    
        ```
        [(ProductId = $NewProduct/ProductId)] 
        ```

    * In the Range field, select **First**

6. Add a Show page activity and set it to open the Edit Product page. Use `NewProductVE` as the page parameter. 
7. Open the ProductOverviewVE_Overview page and add a new button named *New*.
8. In the new button, under On click, select **ACT_CreateProduct**.  

    {{< figure src="/attachments/refguide/modeling/domain-model/view-entities/create-product-microflow.png" >}}
