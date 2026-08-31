# Amazon DynamoDB via AWS CLI

**IAM User Setup, CLI Configuration & Table Operations**

*Lab Documentation*
Prepared by: **Jesswin Anto**

---

## 📌 Objective

Create an IAM user with programmatic (CLI-only, no console) access to Amazon DynamoDB, configure the AWS CLI on a Linux machine using that user's access keys, and then create, list, and populate a DynamoDB table entirely through the AWS CLI.

> **Note**
> This lab demonstrates the recommended way to automate AWS resource management: using a scoped IAM user's access keys with the AWS CLI, rather than performing every action manually through the AWS Console.

---

## Step 1: Create an IAM User (Programmatic Access Only)

An IAM user was created without console access, intended solely for CLI/API operations against DynamoDB.

**Procedure**

1. Navigate to **IAM** in the AWS dashboard → **Users** → **Create user**
2. **User name:** `DynamoDB-User-1`
3. **Permissions options:** Attach policies directly
4. **Permissions policy:** `AmazonDynamoDBFullAccess`
5. Click **Create user**
6. Open the created user → **Security credentials** tab → generate an **Access key**

> **Note**
> The `AmazonDynamoDBFullAccess` managed policy grants full read/write/admin access to DynamoDB only — following the principle of least privilege, this user cannot touch any other AWS service.

*Console screenshots — IAM user and access key creation steps (see original document for images).*

---

## Step 2: Configure the AWS CLI on Linux

The generated access key and secret key were used to configure the AWS CLI on a Linux terminal, linking it to the `DynamoDB-User-1` IAM identity.

**Procedure**

Open a terminal and run:

```bash
aws configure
```

| Prompt | Value |
|---|---|
| AWS Access Key ID | `<value generated in Step 1>` |
| AWS Secret Access Key | `<value generated in Step 1>` |
| Default region name | `ap-south-1` (Asia Pacific — Mumbai) |
| Default output format | `json` |

> **Note**
> Access keys are sensitive credentials — never commit them to source control or share them. Store them securely (e.g., a credentials manager) and rotate them periodically.

---

## Step 3: Create the DynamoDB Table and View/Insert Data

### Create Table

The following command creates a table named `Students` with `StudentId` as the partition (hash) key, using on-demand billing:

```bash
aws dynamodb create-table \
  --table-name Students \
  --attribute-definitions AttributeName=StudentId,AttributeType=S \
  --key-schema AttributeName=StudentId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region ap-south-1
```

> **Note**
> `PAY_PER_REQUEST` billing mode was chosen so DynamoDB automatically scales read/write capacity and charges only for actual requests — ideal for unpredictable or low lab-scale workloads, with no capacity planning required.

*Output — table creation confirmed successfully (see original document for screenshot).*

### View Table

The following command lists all DynamoDB tables in the specified region, confirming that `Students` was created successfully:

```bash
aws dynamodb list-tables --region ap-south-1
```

*Output — listing tables (see original document for screenshot).*

### Add Items

Items (student records) were inserted into the table.

*Output — items added (see original document for screenshot).*

> **Note**
> Since `StudentId` is a String-type partition key with no sort key, each `StudentId` value in the table must be unique — inserting an item with a duplicate `StudentId` will overwrite the existing record.

---

## Conclusion

This lab successfully demonstrates secure, CLI-driven management of AWS resources: a least-privilege IAM user was created with DynamoDB-only permissions, the AWS CLI was configured with its credentials, and a fully functional DynamoDB table was created, verified, and populated — entirely without using the AWS Console for the DynamoDB operations themselves.

---

## 📖 Appendix — DynamoDB Reference Notes

### DynamoDB — Simple Definition

**Amazon DynamoDB** is a fully managed **NoSQL database** service from AWS. Instead of tables with fixed rows/columns like MySQL, it stores data as **key-value pairs** and **JSON-like documents**. It's built for speed and scale — single-digit millisecond response times even with massive amounts of data — and you don't manage any servers; AWS handles all the infrastructure, scaling, and backups.

> Think of it as: *"a giant, super-fast dictionary in the cloud"* where you look things up by a unique key.

### Core Concepts

| Term | Meaning |
|---|---|
| **Table** | Collection of data (like a table in SQL, but schema-less) |
| **Item** | A single record/row in a table |
| **Attribute** | A field/column in an item (e.g., name, age) |
| **Primary Key** | Uniquely identifies an item — either a simple **Partition Key** or a **Partition Key + Sort Key** |

### Important Syntax (AWS CLI Examples)

**1. Create a table**

```bash
aws dynamodb create-table \
  --table-name Users \
  --attribute-definitions AttributeName=UserId,AttributeType=S \
  --key-schema AttributeName=UserId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```

**2. Insert an item (Put)**

```bash
aws dynamodb put-item \
  --table-name Users \
  --item '{"UserId": {"S": "101"}, "Name": {"S": "Jess"}, "Age": {"N": "25"}}'
```

**3. Get an item**

```bash
aws dynamodb get-item \
  --table-name Users \
  --key '{"UserId": {"S": "101"}}'
```

**4. Update an item**

```bash
aws dynamodb update-item \
  --table-name Users \
  --key '{"UserId": {"S": "101"}}' \
  --update-expression "SET Age = :val" \
  --expression-attribute-values '{":val": {"N": "26"}}'
```

**5. Delete an item**

```bash
aws dynamodb delete-item \
  --table-name Users \
  --key '{"UserId": {"S": "101"}}'
```

**6. Query (using Partition Key)**

```bash
aws dynamodb query \
  --table-name Users \
  --key-condition-expression "UserId = :id" \
  --expression-attribute-values '{":id": {"S": "101"}}'
```

**7. Scan (reads whole table — use sparingly, costly on large tables)**

```bash
aws dynamodb scan --table-name Users
```

### Key Data Types in DynamoDB

| Code | Type |
|---|---|
| `S` | String |
| `N` | Number |
| `B` | Binary |
| `BOOL` | Boolean |
| `L` | List |
| `M` | Map (nested JSON object) |
| `SS` / `NS` | String Set / Number Set |
