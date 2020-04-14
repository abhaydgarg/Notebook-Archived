# Constraint

It is used to define rules to allow or restrict what values can be stored in columns. The purpose of inducing constraints is to enforce the integrity of a database.

| Constraint | Description |
| :--- | :--- |
| NOT NULL | It specify that a column can not contain NULL value. |
| UNIQUE | Column must contain unique value. Duplication of value is not allowed. |
| PRIMARY KEY | It enforces the table to accept unique data for a specific column and this constraint creates a unique index for accessing the table faster. |
| FOREIGN KEY | It creates a link between two tables by one specific column of both tables. The specified column in one table must be a PRIMARY KEY and referred by the column of another table known as FOREIGN KEY. |
| CHECK | It determines whether the value is valid or not from a logical expression. |
| DEFAULT | Each column must contain a value ( including a NULL). While inserting data into a table, if no value is supplied to a column, then the column gets the value set as DEFAULT.
