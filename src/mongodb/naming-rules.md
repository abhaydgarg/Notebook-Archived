# Naming rules

## Key

* The `$` character must not be the first character in the key name. Example: `$tags`
* The period `.` character must not appear anywhere in the key name. Example: ta.gs
* The name `_id` is reserved for use as a primary key.

## Collection

* An empty string `" "` cannot be used as a collection name.
* The collection's name must start with either a letter or an underscore.
* The collection name `system` is reserved for MongoDB and cannot be used.
* The collection's name cannot contain the `\0` null character.
