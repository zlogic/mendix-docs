---
title: "Exporting Data with View Entities"
url: /refguide/view-entity-expport-data/

---

## Introduction

Use view entities to export data from your domain model with JSON. 

For this purpose of this use case, the following domain model is used:

{{< figure src="/attachments/refguide/modeling/domain-model/view-entities/export-data/domain-model.png" >}}


## Transform and Export Data with JSON

In this scenario, you need to export a report of customer information in JSON format. The report should contain the customer's billing address and delivery address, all contained in the same field. This can be done in one view entity, rather than having to decide the logic for a Customer-Address association in a microflow.

1. Create a view entity and name it *CustomerVE*. 
2. Add the following query to the entity:

```
SELECT
    c.CustomerId as CustomerID,
    c.CompanyName as Company,
    (c.FirstName + ' ' + c.LastName) as FullName,
    c.Phone as Phone, 
    c.Fax as Fax,
    (ba.Address + ', ' + ba.City + ' ' + ba.PostalCode + ', ' + ba.Country) as BillingAddress,
    (da.Address + ', ' + da.City + ' ' + da.PostalCode + ', ' + da.Country) as DeliveryAddress
FROM Shop.Customer c
    LEFT JOIN c/Shop.BillingAddress/Shop.Address ba
    LEFT JOIN c/Shop.DeliveryAddress/Shop.Address da
```
