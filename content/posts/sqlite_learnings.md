---
title: "SQLite Notes"
date: 2022-08-18T23:36:40-04:00
draft: false
---
SQLite is an embedded database that is [much](https://fly.io/blog/all-in-on-sqlite-litestream/) [loved](https://antonz.org/sqlite-is-not-a-toy-database/) by the developer community for its speed and relative simplicity. I finally got around to using it for a personal project and have documented (and maybe will keep documenting?) some of its idiosyncracies as I learn them along the way.

## Enabling Foreign Key Support ##

From the SQLite [documentation](https://www.sqlite.org/foreignkeys.html):
> Foreign key constraints are disabled by default (for backwards compatibility), so must be enabled separately for each database connection. 

Foreign key constraints can be checked by typing `PRAGMA foreign_keys;`. [^1]

You can turn on foreign key constraints using: `PRAGMA foreign_keys=ON;`.

## Displaying column names in SELECT statements ##

Executing a `SELECT` statement in the SQLite shell does not directly display column names. You can display column names and align them with the results by running the two following commands in the shell: 
- `.headers on`
- `.mode columns`

h/t: StackOverflow user [brother-bilo](https://stackoverflow.com/questions/947215/how-to-get-a-list-of-column-names-on-sqlite3-database#comment122600381_41005204)

[^1]: From the documentation: _If the command "PRAGMA foreign_keys" returns no data instead of a single row containing "0" or "1", then the version of SQLite you are using does not support foreign keys (either because it is older than 3.6.19 or because it was compiled with SQLITE_OMIT_FOREIGN_KEY or SQLITE_OMIT_TRIGGER defined)_. 