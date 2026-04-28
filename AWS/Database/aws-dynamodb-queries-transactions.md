<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Query Data with DynamoDB

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-query)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Query Data with DynamoDB

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-query_733d9399)

---

## Introducing Today's Project!

### What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed, serverless NoSQL database service provided by AWS, designed for high-performance, single-digit millisecond latency at any scale. It is highly useful for modern applications needing seamless scaling and high availability, supporting key-value and document data models for use cases like gaming, IoT, and high-traffic shopping carts.

### How I used Amazon DynamoDB in this project

In today's project, I used Amazon DynamoDB to create tables, load data into those tables using CLI, then applied queries and transactions.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project were the commands — there were more query options than I anticipated.

### This project took me...

This project took me 60 minutes.

---

## Querying DynamoDB Tables

Think of a partition key as the filter that DynamoDB uses to split up and find data.

A sort key is a secondary key used to filter query results further. Sort keys work after the partition key — you still use the partition key to locate the data first, and then the sort key refines the results within that partition.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-query_d105b0b0)

---

## Limits of Using DynamoDB

I ran into an error when I queried for all the comments by user Abdulrahman. This was because a partition key is required to run the query — a limitation that does not exist in relational databases.

Insights we could extract from our Comment table include timestamp-based filtering on comments.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-query_cb3e260c)

---

## Running Queries with CLI

I ran a `get-item` query to retrieve a specific item from a DynamoDB table:

```bash
aws dynamodb get-item \
    --table-name ContentCatalog \
    --key '{"Id":{"N":"202"}}' \
    --projection-expression "Title, ContentType, Services" \
    --return-consumed-capacity TOTAL
```

Additional options I could include are `--consistent-read` for strongly consistent reads.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-query_733d9399)

---

## Transactions

A transaction is a group of operations that all have to succeed — if any single operation in the group fails, none of the changes are applied.

I ran a transaction using `aws dynamodb transact-write-items` with two operations:

1. Add a new comment in the Events forum
2. Update the comment count in the Forum table for Events

```bash
aws dynamodb transact-write-items \
    --transact-items file://transact-items.json
```

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-query_2f65f83e)

---
