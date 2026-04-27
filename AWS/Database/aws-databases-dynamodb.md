

# Load Data into DynamoDB

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-dynamodb)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Load Data into a DynamoDB Table

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-dynamodb_b481c730)

---

## Introducing Today's Project!

### What is Amazon DynamoDB?

Amazon DynamoDB is a non-relational database service.

Non-relational databases use structures other than rows and columns to organise data.

### How I used Amazon DynamoDB in this project

In today's project, I used Amazon DynamoDB to create tables using CLI, load data and manage tables.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project is CLI control.

### This project took me...

This project took me 60 minutes.

---

## Create a DynamoDB Table

DynamoDB tables organise data using partition keys.

In DynamoDB, an attribute is like a piece of data about an item. In this case, our item is Nikko and the attribute is the number of projects Nikko completed.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-dynamodb_a3cefee0)

---

## Read and Write Capacity

Read capacity units (RCUs) and write capacity units (WCUs) provide engines for read or write/update/delete operations respectively.

The Free Tier for DynamoDB gives you 25 GB of data storage, plus 25 Write and 25 Read Capacity Units (WCU, RCU). This is enough to handle 200M requests per month — all for free.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-dynamodb_ef47dd8f)

---

## Using CLI and CloudShell

AWS CloudShell is a shell in your AWS Management Console — a space for you to run code with AWS CLI pre-installed.

AWS CLI (Command Line Interface) is a tool that lets you create, delete, and update AWS resources with commands instead of clicking through the console.

I ran the following CLI commands to create multiple tables at once:

```bash
aws dynamodb create-table \
    --table-name ContentCatalog \
    --attribute-definitions \
        AttributeName=Id,AttributeType=N \
    --key-schema \
        AttributeName=Id,KeyType=HASH \
    --provisioned-throughput \
        ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --query "TableDescription.TableStatus"

aws dynamodb create-table \
    --table-name Forum \
    --attribute-definitions \
        AttributeName=Name,AttributeType=S \
    --key-schema \
        AttributeName=Name,KeyType=HASH \
    --provisioned-throughput \
        ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --query "TableDescription.TableStatus"

aws dynamodb create-table \
    --table-name Post \
    --attribute-definitions \
        AttributeName=ForumName,AttributeType=S \
        AttributeName=Subject,AttributeType=S \
    --key-schema \
        AttributeName=ForumName,KeyType=HASH \
        AttributeName=Subject,KeyType=RANGE \
    --provisioned-throughput \
        ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --query "TableDescription.TableStatus"
```

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-dynamodb_81e0258b)

---

## Loading Data with CLI

I ran the CLI command `aws dynamodb batch-write-item` to write data into the tables.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-dynamodb_791c600b)

---

## Observing Item Attributes

I checked a ContentCatalog item, which had the following attributes: Author, ContentType, Difficulty, Price, and more.

I checked another ContentCatalog item, which had a different set of attributes — including a `video` attribute with a value pointing to the video link.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-dynamodb_b481c731)

---

## Benefits of DynamoDB

**Flexibility** — every item can have its own unique set of attributes, which is a major advantage when items in a table look different from each other. This is not possible with rigid relational database schemas.

**Speed** — DynamoDB uses partition keys to split up a table and quickly locate items, making it significantly faster for key-based lookups than traditional relational databases.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-databases-dynamodb_b481c730)

---
