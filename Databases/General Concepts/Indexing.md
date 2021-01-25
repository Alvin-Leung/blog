# What is database indexing?

An apt metaphor here is the indexing used in books. When we flip to the back of a book, usually there is an index of keywords found within the book. For example, in a programming book, you may be able to search for the term 'refactor' in the index, and find the corresponding page(s) where this term appears.

Likewise, a database index can take the form of a separate table that the DBMS references to quickly find a corresponding table row for a specific value. We can choose which columns are indexed so that the DBMS can quickly find the rows containing the searched values in these columns.

# What problem does indexing solve?

It allows us to quickly find data using filters on the indexed column(s). If we do not have an index on a column, then filtering on that column will result in a full table scan. If we do have an index, then the index can be used, and the query will be much faster in most cases.

For example, say we have the following table:

| Person Table ||
|---|---|
| PK | PersonID: int |
|| FirstName: varchar(40) <br> LastName: varchar(40) <br> Phone-Number: varchar(20) |

We create an index on `LastName`. The following query will use the index and return results fast:

```sql
SELECT * FROM Person WHERE LastName='Thompson';
```

The following query will not use the index, and could be much slower:

```sql
SELECT * FROM Person WHERE FirstName='Brad';
```

# What problems does indexing create?

Much like for the index of a book, when we have a database index, it needs to be updated every time we update or insert a value. This degrades write/update performance.

# What is the difference between a non-clustered and clustered index?

A non-clustered index is like the example of a book index on subjects. It is a separate table that the DBMS can reference to find a row number. We can have multiple non-clustered indices.

A clustered index actually sorts an entire table based on the indexed column(s). It is akin to a phone book, where we have indexed information based on (`LastName`, `FirstName`) and listed phone numbers based on the sorted order which names appear in. We can only have one clustered index since creating a clustered index actually reorders the data. Typically, a clustered index is built on the primary key of a table.

# What is a composite/multi-column index?

This is an index created on two or more columns.

# What are some considerations when choosing which column(s) to index?

- **Do** always index columns which are anticipated to be used in `WHERE` clauses (unless you want a full-text search for your queries...)
- **Do not** index columns that are unused in `WHERE` clauses.
- **Do** index columns which are anticipated to be used as part of `JOIN` operations.

# How does indexing differ between a SQL and NoSQL database?

Here is a comparison between MongoDB and MS SQL Server: http://sql-vs-nosql.blogspot.com/2013/11/indexes-comparison-mongodb-vs-mssqlserver.html