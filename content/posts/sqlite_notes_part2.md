---
title: "SQLite Notes - Part 2"
date: 2022-09-21T00:11:59-04:00
draft: false
---

[Part 1](https://arshpunia.github.io/posts/sqlite_learnings/)

From the [SQLite FAQs](https://www.sqlite.org/faq.html#q19): 
> SQLite will easily do 50,000 or more INSERT statements per second on an average desktop computer. But it will only do a few dozen transactions per second.

In the Python `sqlite3` module, any transaction[^1] needs to be committed before changes are saved in the database ([Source](https://docs.python.org/3/library/sqlite3.html)). This is achieved by calling `con.commit()` on the connection object. 

I ran a quick and dirty test in Python to understand the performance impact of commit frequency. For my low-commit-frequency test (Test 1), I executed 100,000 `INSERT` statements and then committed them. For my high-commit-frequency test (Test 2), a commit was done after every `INSERT` was executed. Whereas the low-frequency test completed almost instantaneously, the high-freqency test was taking too long and I decided to abandon it. Instead, I shifted my focus to see how many commits I can achieve in roughly the same time it took for the low-frequency test to complete. The results were astonishing: 

| Test    | Entries Committed | Time
| ----------- | ----------- |-----------|
| Test 1      | 100,000       |1.782 seconds           |
| Test 2   | 10        |1.748 seconds           |

In approximately the same time-frame, committing after multiple `INSERT` statements as opposed to after every such command, led to a 10,000x increase in write performance[^2]. 

In the event that you are writing multiple rows to a table, it is therefore advisable to _execute_ multiple `INSERT` statements before you commit them (Of course, YMMV). As the FAQs say: 
> The time needed to commit the transaction is amortized over all the enclosed insert statements and so the time per insert statement is greatly reduced. 

To that end, you can also use the `sqlite3` module's `executemany(..)` [method](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.executemany). 



[^1]: An `INSERT` statement implicity opens a transaction
[^2]: Please note that these numbers are the result of a single trial and more runs would need to be conducted to arrive at a more accurate representation of a performance increase.  