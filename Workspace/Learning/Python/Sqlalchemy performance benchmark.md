
## Commit Benchmark

SQLAlchemy provides many ways to manipulate the database. Did a benchmark to insert 300k CashTransaction in different ways,

- ORM commit one by one: 930.96s
    
- ORM add into session one by one, commit together: 28.43s
    
- ORM session.session then commit: 29.97s
    
- ORM Bulk save objects: 11.73s
    
- Core Bulk insert mappings: 7.96s
    
- Core normal insert: 8.85s

## INSERT…ON CONFLICT (Upsert)¶
Starting with version 9.5, PostgreSQL allows “upserts” (update or insert) of rows into a table via the `ON CONFLICT` clause of the `INSERT` statement

---
### 🧠 SQLAlchemy Performance: Bulk Inserts are Key!

Here's a crucial lesson on optimizing database performance with SQLAlchemy, learned from a benchmark inserting 300,000 records.

#### The Knowledge 💡

How you commit data to a database dramatically impacts performance. Committing records one-by-one is incredibly slow compared to bulk operations. For maximum speed, using SQLAlchemy's `Core` layer is often the fastest method.

#### 📊 Benchmark Breakdown

The results speak for themselves. Notice the massive difference between individual commits and bulk methods:

-   **ORM (Commit one-by-one):** 930.96s 🐢
-   **ORM (Single session commit):** ~29s 🏃‍♂️
-   **ORM (Bulk save):** 11.73s 💨
-   **Core (Bulk insert):** 7.96s 🚀

The key takeaway is clear: **avoid committing inside a loop!** The overhead of creating a new transaction for every single insert is a major performance bottleneck.

---

### ✨ Pro Tip: Handling Duplicates with "Upsert"

For databases like PostgreSQL (v9.5+), there's a powerful, native way to handle potential duplicates.

#### What is it? 🤔

It's the `INSERT ... ON CONFLICT` statement, often called an "upsert." This command allows you to either **insert** a new row or **update** it if it already exists, all in a single, atomic operation. This is incredibly efficient for synchronizing data without needing to check for a record's existence first. ✅