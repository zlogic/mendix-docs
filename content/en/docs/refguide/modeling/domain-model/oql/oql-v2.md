---
title: "OQL v2 features"
url: /refguide/oql-v2/
weight: 100
---

## OQL v2 features

In Studio Pro version 10.17, we introduced OQL v2. It is necessary to switch to OQL v2 in order to enable View entities, see [View entities](/refguide/app-settings/#enable-view-entities) app setting.

OQL v2 syntax is not different from OQL. There is a few differences in handling of specific, mostly not very common cases. When switchihng to OQL v2, you should check if existing OQL queries in your model have any of these features.

The following changes are included in OQL v2:

### Handling of duplcate columns in `SELECT`: No longer possible to use duplicate column without entity name

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

### Handling of duplcate columns in `SELECT *` from subquery: allowed to join subqueries with duplicate columns

If both entities `Module.Person` and `Module.City` have a duplicate attribute `Name`, the following query would fail in OQL v1. In OQL v2, it no longer fails, which makes the behavior consistent with the case of `SELECT *` combined with `JOIN` without subqieries.

```sql
SELECT *
FROM (SELECT * FROM Module.Person) P
JOIN (SELECT * FROM Module.City) C
ON P/Residence = C/Name
```

If attribute names are specified explicitly, retrieving duplicate columns with `*` is not allowed, the same as in OQL v1. In this case, use aliases.

The following is **not allowed**:

```sql
SELECT *
FROM (SELECT Name, Residence FROM Module.Person) P
JOIN (SELECT Name FROM Module.City) C
ON P/Residence = C/Name
```

But it is allowed if aliases are used:

```sql
SELECT *
FROM (SELECT Name AS PersonName, Residence FROM Module.Person) P
JOIN (SELECT Name AS CityName FROM Module.City) C
ON P/Residence = C/CityName
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

In OQL v1, Runtime would pass such query to the database, and it woulkd be handled by most database engines except SQL Server.

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

For view entities, having `ORDER BY` without `LIMIT` and/or `OFFSET` is not allowed even for the top level query. Similarly to the case of subqueries, view entitiy results are not accessed directly, but are often subject of further filtering and ordering. TRherefore, it would not make sense to have `ORDER BY` directly in the view entity definition unless it is used to select a slice of the result by the means of `LIMIT` and/or `OFFSET`.

### Changes in result type handling of arithmetic functions

OQL v2 handles differently the situations where different sides of an arithmetic operation have different data types. For example,

```sql
SELECT Attribute1 + Attribute2 AS SumAttr
FROM Module.Entity
```

In OQL v1, the result of the arithmetic operation will always be of type pf the first attribute in the expression. We would not handle type differences and instead would let the database handle it. Therefore, the result would depend on the underlying database engine.

OQL v2 hjas more control over arithmetic operations. For numeric types (Integer, Long and Decimal), the result of the operation is always of the type pf the most precise attribute type in  the following precedence:

- Decimal (highest)
- Long
- Integer

If any side of the operation is of a non-numeric type, no casting is performed, and we let the database handle such operation, the same as OQL v1.

### The result type of `ROUND` is now `Decimal`

In OQL v1, `ROUND` would return a result in nlo longer supported by Studio Pro type `Float`. In OQL v2, it always returns `Decimal`.

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