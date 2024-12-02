Trigger-based
- uses database functions that executes automatically when specific changes (INSERT, UPDATE, DELETE) occur

| pros                                              | cons                                                                                     |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| uses database builtin functionality and auditable | creates an overhead to the database for each transaction with a trigger                  |
| precise and real time                             | may require a separate table/storage for the cdc data to reduce the load on the database |

Log-based
- reads changes from database transaction logs (e.g; WAL)

| pros                                                                                | cons                             |
| ----------------------------------------------------------------------------------- | -------------------------------- |
| high efficiency with minimal performance impact as it uses the file system directly | database specific implementation |

Query-based
- use custom queries to periodically poll the database for changes using timestamps or change indicators

| pros                                               | cons                                                  |
| -------------------------------------------------- | ----------------------------------------------------- |
| flexible and simple to implement                   | add read loads to the database from running the query |
| does not require direct access to logs or triggers | may require manual querying and not be real time      |

Replication
- use transaction logs to copy database for real-time data replication

| pros                                                  | cons                                   |
| ----------------------------------------------------- | -------------------------------------- |
| precise and real time                                 | may not be possible for some databases |
| less overhead as it accesses the file system directly | complex logic                          |
