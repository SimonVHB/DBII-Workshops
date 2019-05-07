# Workshop - Triggers
In this workshop you'll learn how to create and use a Trigger.

## Prerequisites
- A running copy of database **xtreme**;

## Exercise 1 
> Currently not supported, but will be supported shortly. For now see the Oefeningenbundel via Chamillo. Normally this exercise should already be completed.

---

## Exercise 2
To keep track of all the modifications done to the `product` table, we want to create a separate table to check who did what at which specific time (also referred as an audit table). To make sure this is persisted in a uniformal way, we're going to use a `trigger`. The trigger should log a new record in the `ProductAudit` table when a mutation of the `product` table has taken place.

### Call to action
- Create a new table called `ProductAudit` with the following columns:
    - Id - Primary Key
        - Identity
    - UserName - nvarchar(128)
        - Default SystemUser
    - CreatedAt - DateTime
        - Default UTC Time
    - Operation - nchar(6)
        - The name of the operation we performed on a row
            - Updated
            - Created
            - Deleted
- If the table is already present, drop it.
- Create a trigger for all actions (Update, Delete, Insert) to persist the mutation of the `product` table.
- Use system functions to populate the `UserName` and `CreatedAt`.

### Execution
Make sure the following code can be executed:

```sql
INSERT INTO Product(productID, productName)
VALUES(12, 'New product12')

UPDATE Product
SET productName = 'abc'
WHERE productID = 12

DELETE FROM product
WHERE productID = 12

SELECT * FROM productAudit -- Changes should be seen here.
```

### Deep Dive
1. What are some possible issues having a lot of triggers?
2. What is the difference between `GETDATE()` and `GETUTCDATE()`?

### Solution
A possible solution of exercise 2 can be found [here](/solutions/triggers-2.sql)

---

## Exercise 3
For this exercise we'll introduce a new (redundant) attribute/column for the `ProductType` table called `AmountOfProducts` (int). The column will keep track of all the `products` that have this specific `ProductType`. Doing so the amount a `producttype` is linked to `products` does not have to be recalculated each time on a `SELECT` query but should be updated each time a `product` is `mutated`(Deleted, Inserted or Updated). For this overhead we'll use a `trigger` on the `product` table.

> Redundent data is not always a bad idea, it can speed up the performance drastically when done correctly, since you often read data a lot more than you write it. However this can create some additional complexity or overhead in your database, so be cautious when introducing redundant columns. 

### Call to action
- Add a new column to the table `producttype`:
    - AmountOfProducts - int
- Update all the rows of the `producttype` table to reflect the actual `AmountOfProducts`.
    - Write an update query for this.
- Create a trigger for all actions (Update, Delete, Insert) to reflect the possible mutation of the `product` table, so the `AmountOfProducts` attribute of all the `producttype` rows are correct.

### Execution
Make sure the following code can be executed:

```sql
INSERT INTO Product(ProductID, ProductName, ProductTypeID) 
VALUES(61, 'New product 61',1)
-- ProductType's AmountOfProducts  should be updated
DELETE FROM product WHERE productId = 60
-- ProductType's AmountOfProducts  should be updated
```

### Solution
A possible solution of exercise 3 can be found [here](/solutions/triggers-3.sql)
