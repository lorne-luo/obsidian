# Issue

On GHA integration testing,  `alembic check` command always detected changeset, even we just run `alembic upgrade head` to apply the newest migration scripts.

# Reason

Before I thought `actions/checkout@v4` will checkout the target branch directly, but it's not.

It looks when `actions/checkout@v4`  works for pull\_request trigger

```
on:
  pull_request:
    branches: [ main ]
```

it's not checkout feat branch directly, it will do:

* checkout feat branch
* **merge feat branch with origin/main**

So all the **changes in main branch will also included**.

the `actions/checkout@v4` will check out main first, then merge the feature branch into mian and run the test

```
      - name: Checkout code
        uses: actions/checkout@v4
```

The difference between test on local and GHA

| test on local         | test on GHA                    |
| --------------------- | ------------------------------ |
| run on feature branch | feature branch rebased on main |

# Solution

Simply rebase this feature branch with `origin/main` will solve this issue
