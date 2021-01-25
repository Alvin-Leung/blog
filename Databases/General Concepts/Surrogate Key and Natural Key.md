# What is a natural key?

This is a type of primary key that is also a natural descriptor of data in a table row. For example, if you had a **Users Table**, then a `username` (something that a user is required to enter when signing up to use your application) could be a natural key, given all usernames in the system are restricted to be unique.

# What is a surrogate key?

This is a type of primary key that does not naturally occur in the data in a table row. For example, if you had a **Users Table**, but no other unique columns (i.e. the `username` natural key is not present) are available, you could create a surrogate key like `user_id`. This could hold auto-incrementing integer values with every record added. 

The main difference from the natural key is that a surrogate key has no meaning outside the confines of your database and application.

# When should you use a natural key? A surrogate key?

| Primary Key Type | Pros | Cons |
| --- | --- | --- |
| Natural | No need to add extra column, as with surrogate keys | Sometimes there is no good option for a natural key. As with any primary key, the value must be unique, unchanging, and non-null.<br><br>Because natural keys are coupled to real world meaning, future business requests to change application behavior could force these keys to change. This may involve having to change the matching foreign keys in many other tables. |
| Surrogate | Easy to add<br><br>No need to worry about future business requirements impacting your surrogate key values; their meaning is confined to the database. | You have to add a column to your table. In some cases, this could be avoided through the selection of an overlooked natural key.<br><br>Because surrogate keys are typically auto-incrementing numbers or randomly generated unique values, the relationship between tables is slightly obfuscated (*"Hmm, what does the foreign key `89da6158-883f-4dae-90d7-030fada1d5f8` refer to?"*) |


> Tip: Try to use either **only** natural keys or **only** surrogate keys for a database. Having a mix of the two is slightly confusing for anyone perusing multiple tables within a database (*"Ok, now does this table use a surrogate or natural key?"*).