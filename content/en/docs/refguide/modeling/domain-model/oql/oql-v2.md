---
title: "OQL Version 2 Features"
linktitle: "OQL V2 Features"
url: /refguide/oql-v2/
weight: 100
---

## Introduction

In Studio Pro version 10.17, we introduced OQL v2. You will have to switch to OQL v2 in order to enable View entities. See the [View entities](/refguide/app-settings/#enable-view-entities) app setting for more information.

OQL v2 syntax is the same as OQL. However, there are a few differences in the handling of specific, mostly not very common, cases.

When switching to OQL v2, you should check if existing OQL queries in your model have any of these features.

## OQL V2 Features

The following changes are included in OQL v2:

### `GROUP BY` Association Is No Longer Allowed

In OQL v2, you can no longer use a path over association in a `GROUP BY` query because the outcome of such query was unpredictable in the case of one-to-many and many-to-many associations.

This query is not allowed anymore:

```sql
SELECT COUNT(*)
FROM Module.Person
GROUP BY Module.Person/Module.Person_City/Module.City/Name
```

Instead, you can use the long path in the `JOIN` pattern as follows:

```sql
SELECT COUNT(*)
FROM Module.Person AS P
JOIN Module.Person/Module.Person_City/Module.City AS C
GROUP BY C/Name
```

### More Strict Data Type Validation in OQL Functions

{{% todo %}}this will be added when implemented, as there are ambiguities{{% /todo %}}

### Subquery Columns Should Have a Name or an Alias

In OQL v1, when a column in the `SELECT` clause is a subquery, it was possible for it not to have a name. For example:

```sql
SELECT
  Name,
  (
    SELECT COUNT(*)
    FROM Module.Person
    WHERE Module.Person/City = Module.City/Name
  )
FROM Module.City
```

In OQL v2, such queries are no longer allowed. You must always provide an alias for subqueries that do not result in a named column. The query above can be rewritten as follows:

```sql
SELECT
  Name,
  (
    SELECT COUNT(*)
    FROM Module.Person
    WHERE Module.Person/City = Module.City/Name
  ) AS PersonCount
FROM Module.City
```

### Duplicate Columns in ‘SELECT’

#### Specify Entity Name

 You can no longer use duplicate column names without specifying an entity name when it is not clear which entity the attribute belongs to. You must explicitly specify the entity.

For example, if both entities `Module.Person` and `Module.City` have an attribute `Name`, you cannot use the following query in OQL v2: 

```sql
SELECT Name
FROM Module.Person
JOIN Module.Person/Module.Person_City/Module.City
```

In OQL v1, the query would assume that `Name` belongs to `Module.Person`, which could become a source of errors.

In OQL v2, you can write the same query specifying the entity, as follows:

```sql
SELECT Module.Person/Name
FROM Module.Person
JOIN Module.Person/Module.Person_City/Module.City
```

#### Allow Joining of Subqueries with Duplicate Columns

In OQL v1, if both entities `Module.Person` and `Module.City` have a duplicate attribute `Name`, the following query would fail:

```sql
SELECT *
FROM (SELECT * FROM Module.Person) P
JOIN (SELECT * FROM Module.City) C
ON P/Residence = C/Name
```

In OQL v2, the query above no longer fails, which makes the behavior consistent with the case of `SELECT *` combined with `JOIN` without subqueries.

Having concrete duplicate attribute names is also now allowed:

```sql
SELECT *
FROM (SELECT Name FROM Module.Person) P
JOIN (SELECT Name FROM Module.City) C
ON P/Residence = C/Name
```

You still cannot use duplicate aliases in different joined subqueries. This leads to an error the same way as in OQL v1.

For example, you cannot use the following query:

```sql
SELECT *
FROM (SELECT LastName AS N FROM Module.Person) P
JOIN (SELECT Name AS N FROM Module.City) C
ON P/Residence = C/Name
```

### `ORDER BY` in Subquery

You must now have a `LIMIT` and `OFFSET` in subquery containing `ORDER BY`. Using `ORDER BY` in subquery makes sense only when it is combined with `LIMIT` and `OFFSET`. Without the limitations, database engines do not guarantee that the row order in the subquery will be preserved in the outer query.

Consequently, you cannot use the following query in OQL v2.

```sql
SELECT *
FROM (
    SELECT Name
    FROM Module.Person
    ORDER BY Name
)
```

In OQL v1, Runtime would have passed such query to the database and. although it would have been handled by most database engines, it could not be handled by SQL Server.

You can still use a subquery with `LIMIT` and/or `OFFSET`. For instance, in the following query, the subquery returns the first 20 objects of entity `Module.Person` ordered by `Name`:

```sql
SELECT *
FROM (
    SELECT Name
    FROM Module.Person
    ORDER BY Name
    LIMIT 20
)
```

### `ORDER BY` in View Entities

For view entities, you must now have a `LIMIT` and `OFFSET` in all `ORDER BY` clauses, even for the top level query.

This is because view entity results are not accessed directly. In the Runtime, a View entity is wrapped inside another `SELECT` query as a subquery, which allows further filtering and ordering. This makes it similar to the Subquery case, above.

### Result Types from Arithmetic Functions

OQL v2 handles the situations where different sides of an arithmetic operation have different data types differently.

Take the following example:

```sql
SELECT Attribute1 + Attribute2 AS SumAttr
FROM Module.Entity
```

In OQL v1, the result of the arithmetic operation will always be of type pf the first attribute in the expression because it is handled by the database. Therefore, the result would depend on the underlying database engine.

When handling numeric types in OQL v2 (Integer, Long, and Decimal), the result of the operation is always the most precise attribute type, using the following precedence:

* Decimal (highest)
* Long
* Integer

If any side of the operation is of a non-numeric type, no casting is performed, and the result is handled by the , as in OQL v1. See [Expression syntax](/refguide/oql-expression-syntax/#type-coercion) for more information.

### The Result Type of ‘ROUND’ Is Now ‘Decimal’

In OQL v1, `ROUND` would return a `Float` result. 'Float' is no-longer supported by Studio Pro. In OQL v2, it always returns `Decimal`.

### Attribute Alias in `WHERE`

In OQL v1, referring to an attribute by alias was allowed, but it would throw an exception in the database. In OQL v2, that is no longer allowed.

### `JOIN` Without `ON`

In OQL v1, it was possible to write a query such as the following.:

```sql
SELECT *
FROM Module.Person
JOIN Module.City
```

This is no longer allowed in OQL v2 as a query like this would fail in every supported database engine except MySQL.

In OQL v2, you must either specify `ON` or use join over association. So you could rewrite the query above as follows:

```sql
SELECT *
FROM Module.Person, Module.City
```

### ‘JOIN’ an Entity to Its Own Generalization

In OQL v1 there was a bug which meant that when you ‘JOIN’ed an entity to its own generalization it would generate unexpected specialization columns. This has been fixed.

For example, say entity `Module.Vehicle` has two specializations: `Module.Car` and `Module.Bike`. The query below would previously  generate unexpected duplicate columns for entity `Vehicle`. This no longer happens.

```sql
SELECT *
FROM Module.Car
JOIN Module.Vehicle
ON True
```
