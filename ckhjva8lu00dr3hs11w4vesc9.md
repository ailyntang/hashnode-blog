## SQLite learnings

# SQLITE != DONE

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

# Random learnings

## Inserting rows: start at 1 not 0

In the bind statement, you need to specify an integer, which I assume is the column index. 

`sqlite3_bind_int(insertStatement, <Insert an integer here>, Int32(id))`

So below, I have a table with 3 columns. First column is the id. Normally I'd assume the integer I need to insert in the bind statement is the column index. So for the id column, I would insert 0. However, the number needs to be 1, not 0.
 

```
CREATE TABLE IF NOT EXISTS sleep(
        id INTEGER PRIMARY KEY,
        start INTEGER NOT NULL,
        end INTEGER NOT NULL);

INSERT INTO sleep (id, start, end) VALUES (?, ?, ?);

sqlite3_bind_int(insertStatement, 1, Int32(id)) // You can leave this out, and SQL will add it for you
sqlite3_bind_int(insertStatement, 2, Int32(start))
sqlite3_bind_int(insertStatement, 3, Int32(end))
```

## Primary key: leave it out and SQL manages it for you

Typically you'd use something like a customer id that is unique, but for my sleep data, I don't have that.

In the code above, if you don't include `sqlite3_bind_int(insertStatement, 1, Int32(id))`, then SQL will generate a unique primary key id for you, starting at 1.

This is great for me, as I can't guarantee that the start and end dates will be unique, so they aren't be my primary key.

Also I don't want to be managing ids myself by checking the last id then incrementing it by one. SQL will do all this for me if I don't bind anything to the id field.

## Read rows: start column index at 0 as expected

When reading, the column index starts from 0 as you would expect.

This makes me think I've misunderstood what the binding integer is.

```
func readAll() -> [SleepSession] {
        let queryStatementString = "SELECT * FROM sleep;"
        var queryStatement: OpaquePointer? = nil
        var sleepSessions : [SleepSession] = []
        
        if sqlite3_prepare_v2(db, queryStatementString, -1, &queryStatement, nil) == SQLITE_OK {
            while sqlite3_step(queryStatement) == SQLITE_ROW {
                let id = sqlite3_column_int(queryStatement, 0)
                let start = sqlite3_column_int(queryStatement, 1)
                let end = sqlite3_column_int(queryStatement, 2)
                sleepSessions.append(SleepSession(start: Int(start), end: Int(end)))
                print("Query Result: \(id): \(start) | \(end)")
            }
        } else {
            print("Read all: SELECT statement could not be prepared")
        }
        sqlite3_finalize(queryStatement)
        return sleepSessions
    }
```

