## What is a foreign key?

A reference to a primary key on another table. For example, let's say we have a university enrollment application with the following tables:

| UniversityClass Table | |
| --- | --- |
| PK | ClassID |
| | InstructorID: int <br> BuildingID: int <br> |

| Instructor Table | |
| --- | --- |
| PK | InstructorID |
| | Name: varchar(30) <br> Email: varchar(50) <br> |

| Building Table | |
| --- | --- |
| PK | BuildingID |
| | Name: varchar(30) <br> Email: varchar(50) <br> |

The `InstructorID` and `BuildingID` on the child/associated **UniversityClass Table** are foreign keys in this case; they reference the primary key of records on the primary/parent **Instructor Table** and **Building Table**.

## When would you use the `NOT NULL` constraint with foreign keys?

The `NOT NULL` constraint can be enforced on columns where you want the database to reject row add/update operations that set the column value to `NULL`. Referencing the above example, if every class *must* have an assigned building to be valid, we could enforce the `NOT NULL` constraint on the `BuildingID` foreign key column in the **UniversityClass Table** 

## What is a foreign key constraint and what is it used for?

Foreign key constraints are used to preserve [referencial integrity](https://database.guide/what-is-referential-integrity/), and enforce specific parent-child relationship logic that should automatically occur. 

> Note: All below examples use foreign constraints and actions from MySQL. Different RDBMS may have alternate names for these concepts (but the idea is the same).

Continuing on with the above example, we might want to automatically set corresponding `InstructorID` foreign keys on the **UniversityClass Table** to `NULL` when we delete an instructor from the **Instructor Table**. We can do this by setting an `ON DELETE` foreign key constraint on the **Instructor Table** with an action of `SET NULL`.

As another example, we might want to automatically delete all classes contained in a building that is deleted. We can once again set the `ON DELETE` foreign constraint, but this time on the **Building Table** and with a `CASCADE` action. The delete of the building record cascades down to the classes that take place in that building. 

Alternatively, another action we could apply here instead is `RESTRICT`, if we want to ensure that the database blocks deleting of buildings that have classes in them. 

> Note that foreign key constraints are always set on the **parent** table in a foreign key relationship.

Examples of foreign key constraints:
- `ON DELETE`
- `ON UPDATE`

Examples of actions on constraints:
- `RESTRICT`
- `CASCADE`
- `SET NULL`

Example with SQL:

Here, we are creating a parent `products` table, and a child `inventory` table. `inventory.product_id` is a foreign key that points to `products.product_id`. When a product row is deleted, any inventory row with an `inventory.product_id` matching the deleted product item's primary key will have it's `inventory.product_id` nullified.

```sql
CREATE TABLE products
( product_id INT PRIMARY KEY,
  product_name VARCHAR(50) NOT NULL,
  category VARCHAR(25)
);

CREATE TABLE inventory
( inventory_id INT PRIMARY KEY,
  product_id INT,
  quantity INT,
  min_level INT,
  max_level INT,
  CONSTRAINT fk_inv_product_id
    FOREIGN KEY (product_id)
    REFERENCES products (product_id)
    ON DELETE SET NULL
);
```

Source: https://www.techonthenet.com/sql_server/foreign_keys/foreign_delete_null.php