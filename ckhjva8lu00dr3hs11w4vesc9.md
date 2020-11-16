## SQLite learnings

If you are getting the issue where the step statement doesn't equal `SQLITE_DONE`, then there are a few reasons.

```
if sqlite3_step(deleteStatement) == SQLITE_DONE {
                print("All rows deleted from table")
            } else {
                print("Unable to delete all rows from the table")
            }
```

Likely causes of the issue:
* Primary key is not unique
* Using incorrect column ints for binding. For some reason, the first column needs to be 1 and not 0. `sqlite3_bind_int(insertStatement, 2, Int32(age))` (where age is the second column)
* The statement needs to be finalized in the all SQL statements you execute. If the prior SQL statement is not finalized, then this error will occur.