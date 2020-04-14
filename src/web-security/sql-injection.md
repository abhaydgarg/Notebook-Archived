# SQL injection

## Prevention

### Parameterized Statements

Programming languages talk to SQL databases using database drivers. A driver allows an application to construct and run SQL statements against a database, extracting and manipulating data as needed. Parameterized statements make sure that the parameters (i.e. inputs) passed into SQL statements are treated in a safe manner.

```php
<?php
// Bad, bad news! Don't construct the query with string concatenation.
$sql = "SELECT * FROM users WHERE email = '" + email + "'";
$results = query.execute(sql);

// Construct the SQL statement we want to run, specifying the parameter.
$sql = "SELECT * FROM users WHERE email = ?";
$results = query.execute(sql, email);
```

### Object Relational Mapping

Many development teams prefer to use Object Relational Mapping (ORM) frameworks to make the translation of SQL result sets into code objects more seamless. ORM tools often mean developers will rarely have to write SQL statements in their code – and these tools thankfully use parameterized statements under the hood.
