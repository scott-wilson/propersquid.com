+++
title = "Asset Publish"
date = 2023-10-11
draft = true
description = "A library for publishing assets or shots for production."
template = "project_page.html"

[taxonomies]
categories = ["Programming"]
tags = ["programming", "python", "rust", "c", "pipeline"]

[extra]
url = "https://github.com/scott-wilson/checks"
+++

TODO: The asset publish framework needs to be redesigned - The transactions have
no way of getting data outside of the transaction.

Overview
--------

This framework is designed to provide a way to publish data. This may include
saving data to the filesystem, or updating metadata in a database.

Example
-------

### Transactions

```python
# Database transaction
from typing import Optional

import pypublish


class DatabaseTransaction(pypublish.transactions.Transaction):
    def __init__(self, conn: Connection, name: str, publish_type: str) -> None:
        self.__conn = conn
        self.__name = name
        self.__publish_type = publish_type
        self.__publish = None

    def value(self) -> Optional[Publish]:
        return self.__publish

    async def commit(self) -> None:
        self.__publish = self.__conn.create_publish(self.__name, self.__publish_type)

    async def rollback(self) -> None:
        self.__conn.delete(self.__publish)
        self._publish = None
```

### Publishes

```python
import pypublish


class MyPublish(pypublish.Publish):
    def __init__(self, asset: Asset) -> None:
        self.__asset = asset
        self.__publish = None

    async def pre_publish(
        self, transaction: pypublish.transactions.RootTransaction, context: int
    ) -> int:
        txn = DatabaseTransaction(1)
        transaction.add_child(txn)

        ctx = context + 1
        self.values.append(("pre_publish", ctx))
        return ctx

    async def publish(
        self, transaction: pypublish.transactions.RootTransaction, context: int
    ) -> int:
        txn = TestTransaction(2)
        transaction.add_child(txn)

        ctx = context + 2
        self.values.append(("publish", ctx))
        return ctx

    async def post_publish(
        self, transaction: pypublish.transactions.RootTransaction, context: int
    ) -> int:
        txn = TestTransaction(3)
        transaction.add_child(txn)

        ctx = context + 3
        self.values.append(("post_publish", ctx))
        return ctx
```

Features
--------

- A Rust, C, and Python 3 API
- A pre-publish, publish, post-publish step
- A transaction system for performing filesystem, database, etc actions
- Rolling back transactions if failed

Requirements
------------

- Make
- Rust: 1.66 or later (This is not the guaranteed minimum supported Rust
  version)
- Python: 3.7 or later
- Poetry

Install
-------

```bash
cd /to/your/project
cargo add --git https://github.com/scott-wilson/publish.git
```

Design
------

### Transactions

Transactions are responsible for making publishes permanent. They could be used
for filesystem operations such as copying, moving, or hard linking. Or, they
could be used for registering a publish entity on the database.

### Publishes

Publishes are a collection of transactions and data transformers. The publishes
have the following stages:

- Pre-publish: This is used to prepare a publish. It could include registering a
publish entity on the database, and creating a directory to publish to.
- Publish: This is the main body of work. For example, optimizing assets for
  publishes, generating caches, etc. Then, saving publishes to the publish
  directory.
- Post-publish: This is for finalizing the publish. For example, marking the
  publish entity as ready, locking the directory, etc.

### Runner

The runner will run the publish and return the final result. If any of the
publish stages fail, then it will try to roll back the transactions that have
run.
