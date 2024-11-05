---
title: "OQL v2 features"
url: /refguide/oql-v2/
weight: 100
---

## OQL v2 features

In Studio Pro version 10.17, we introduced OQL v2. It is necessary to switch to OQL v2 in order to enable View entities, see [View entities](/refguide/app-settings/#enable-view-entities) app setting.

OQL v2 syntax is not different from OQL. There is a few differences in handling of specific, mostly not very common cases. When switching to OQL v2, you should check if existing OQL queries in your model have any of these features.

The following changes are included in OQL v2:

### `GROUP BY` association is no longer allowed

In OQL v2, is is no longer possible to use a path over association in a `GROUP BY` query because the outcome of such query was unpredictable in case of one-to-many and many-to-many associations.

This query is not allowed anymore:

```sql
SELECT COUNT(*)
FROM Module.Person
GROUP BY Module.Person/Module.Person_City/Module.City/Name
```

Instead, you can use the long path in `JOIN` pattern the following way:

```sql
SELECT COUNT(*)
FROM Module.Person AS P
JOIN Module.Person/Module.Person_City/Module.City AS C
GROUP BY C/Name
```

### More strict data type validation in OQL functions

! TODO: this will be added when implemented, as there are ambiguities

### Subquery columns should have a name or an alias

When a column in the `SELECT` clause is a subquery, it is possible for it not to have a name. For example:

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

In OQL v2, such queries are no longer allowed. An alias should always be provided for subqueries that do not result in a named column:

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

### Handling of duplicate columns in `SELECT`: No longer possible to use duplicate column without entity name

It is no longer allowed to select an attribute name when it is not clear which entity the attribute belongs to.

For example, if both entities `Module.Person` and `Module.City` have an attribute `Name`, the following query is **no longer allowed**: 

```sql
SELECT Name
FROM Module.Person
JOIN Module.Person/Module.Person_City/Module.City
```

In OQL v1, we would assume that `Name` belongs to `Module.Person`, which could become a source of errors. In OQL v2, in case of duplicate attribute names, the entity should be explicitly specified:

```sql
SELECT Module.Person/Name
FROM Module.Person
JOIN Module.Person/Module.Person_City/Module.City
```

### Handling of duplicate columns in `SELECT *` from subquery: allowed to join subqueries with duplicate columns

If both entities `Module.Person` and `Module.City` have a duplicate attribute `Name`, the following query would fail in OQL v1. In OQL v2, it no longer fails, which makes the behavior consistent with the case of `SELECT *` combined with `JOIN` without subqueries.

```sql
SELECT *
FROM (SELECT * FROM Module.Person) P
JOIN (SELECT * FROM Module.City) C
ON P/Residence = C/Name
```

Having concrete duplicate attribute names is also now allowed:

```sql
SELECT *
FROM (SELECT Name FROM Module.Person) P
JOIN (SELECT Name FROM Module.City) C
ON P/Residence = C/Name
```

But using duplicate aliases in different joined subqueries leads to an error the same way as in OQL v1. The following is **not allowed**:

```sql
SELECT *
FROM (SELECT LastName AS N FROM Module.Person) P
JOIN (SELECT Name AS N FROM Module.City) C
ON P/Residence = C/Name
```

### `ORDER BY` without `LIMIT` and `OFFSET` in subquery is no longer allowed

Using `ORDER BY` in subquery makes sense only when it is combined with `LIMIT` and `OFFSET`. Without the limitations, the database engines do not guarantee that the row order in the subquery will be preserved in the outer query.

That being said, in OQL v2, the following is **no longer allowed**:

```sql
SELECT *
FROM (
    SELECT Name
    FROM Module.Person
    ORDER BY Name
)
```

In OQL v1, Runtime would pass such query to the database, and it would be handled by most database engines except SQL Server.

A subquery with `LIMIT` and/or `OFFSET` is still allowed. For instance, in the following query, the subquery returns first 20 objects of entity `Module.Person` ordered by `Name`:

```sql
SELECT *
FROM (
    SELECT Name
    FROM Module.Person
    ORDER BY Name
    LIMIT 20
)
```

### `ORDER BY` without `LIMIT` and `OFFSET` in View Entity is not allowed

For view entities, having `ORDER BY` without `LIMIT` and/or `OFFSET` is not allowed even for the top level query. Similarly to the case of subqueries, view entity results are not accessed directly. In Runtime, a View entity is wrapped inside another `SELECT` query as a subquery, which allows further filtering and ordering, which makes it similar to the Subquery case.

### Changes in result type handling of arithmetic functions

OQL v2 handles differently the situations where different sides of an arithmetic operation have different data types. For example,

```sql
SELECT Attribute1 + Attribute2 AS SumAttr
FROM Module.Entity
```

In OQL v1, the result of the arithmetic operation will always be of type pf the first attribute in the expression. We would not handle type differences and instead would let the database handle it. Therefore, the result would depend on the underlying database engine.

OQL v2 has more control over arithmetic operations. For numeric types (Integer, Long and Decimal), the result of the operation is always of the type pf the most precise attribute type in  the following precedence:

* Decimal (highest)
* Long
* Integer

If any side of the operation is of a non-numeric type, no casting is performed, and we let the database handle such operation, the same as OQL v1. See [Expression syntax](/refguide/oql-expression-syntax/#type-coercion)

### The result type of `ROUND` is now `Decimal`

In OQL v1, `ROUND` would return a result in the no-longer supported by Studio Pro type `Float`. In OQL v2, it always returns `Decimal`.

### Attribute alias in `WHERE`

In OQL v1, referring to an attribute by alias was allowed, but it would throw an exception in the database. In OQL v2, that is no longer allowed.

### `JOIN` without `ON`

In OQL v1, it was possible to write a query like the following. That is **no longer allowed** in OQL v2:

```sql
SELECT *
FROM Module.Person
JOIN Module.City
```

Such query would fail in every supported database engine except MySQL. Now we require to either specify `ON` or to use join over association.

The query above can be rewritten as:

```sql
SELECT *
FROM Module.Person, Module.City
```

### `JOIN` of an entity to its own generalization does not generate unexpected specialization columns anymore

Let's say Entity `Module.Vehicle` has two specializations: `Module.Car` and `Module.Bike`. The query below would generate unexpected duplicate columns for entity `Vehicle`. We fixed that bug.

```sql
SELECT *
FROM Module.Car
JOIN Module.Vehicle
ON True
```
