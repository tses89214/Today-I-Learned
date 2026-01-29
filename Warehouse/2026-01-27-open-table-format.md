## Metadata
- Reading Date: 2026-01-27 - 2026-01-28
- Post: [Why do we need open table formats like Delta Lake or Iceberg?](https://medium.com/data-engineer-things/why-do-we-need-open-table-formats-like-delta-lake-or-iceberg-cee21fed4592)
- Posted Date: 2025-12-20
- Tags: Warehouse, Open Table Format, Delta Lake

## Key Takeaways

### History class
In the past, people suggested that we should have two layers in our data system: one for raw data, we called it lake layer; and another one is for clean data which we called it warehouse layer.
Today, if you ask like "what should I design my data system", you will find more and more discussion about "Lakehouse" patterns.

In the earliest times like 1990s, you were the first member of data team, the scale of business is small, you can query data from OLTP data directly to get a report and done your task. But with business grows, people started to use third-party data, like something-ads, something-data-trcking systems, and so on. You will find it's harder to get data directly and immediately because the policy, rate limitation or resource issues. So, You started to schedule and run these query at night, and stored data into a shared-level database, so that you can deal with them while you are at work. 

Time flies we arrived 2000s, we have Apache Hadoop, an object storage that you can store not only csv or txt files but also media, music and anything. People threw all their data into it and called it "data lake". And soon it became a "data swamp", unorganized and messy.

So, people dived into building pipelines, clean and transfer messy data lake into clean "Data Warehouse".
People can find data smoothly when they need finally.

### Lakehouse Came to World
In 2009, Meta released a paper introducing "Apache Hive", and open source query engine building on Hadoop, which means: you can query structured data directly from the lake, and data stayed in the same place, You do not need to transfer it from lake to warehouse.

Soon, tools and services has similar idea show up, like Impala, Dermel, Trino, Presto, and so on.
All of them are dealing with the same problems that how to query data faster with less cost on storage and transfer.

In 2020, Databricks introduce the concept "Data Lakehouse" officially. It stated that the two-layers structure has some problems, like maintaining the consistency between lake and warehouse is challenging
and costly, the lake and warehouse will double your storage cost, etc. And they believed these problems can be resolved by Lakehouse structures.

However, Lakehouse has its own challenge to overcome. The most challenging problems is the abstraction between lake and warehouse is mismatch. So when we try to build warehouse-like layer on lakes, we will meet some problems like missing proper ACID controls, and lake is weak on data discovery and DML query.
Thus, Databricks suggested that a lakehouse must have a metadata layer sit between them, which provides robust table abstraction information, solving most problems we discussed.

### Problems of Apache Hive 
1. Relational Databases guarantee ACID transactions, but hive don't. A file may corrupt during transfer if network break. 
2. When multiple users write data into a table concurrently, Relational DB can handle it with locks, but hive don't.
3. Hive have to list all files before read it, But it may use lots of time to finish it, even if users only query a few partitions.
4. In hive, data use similar prefix will be put together, which means in extreme cases, all requests will be sent to the same server, then bottleneck show up and cause performance degrade.

### What Tech Giants Do
1. Hudi
- Uber created it to handle incremental data refresh. Uber's engineer noticed that they have to re-ingest all 120 TB data every 8 hour, but only 1% of data changed. So they invent Hudi. Data will be split into two parts, one is base file(parquet), another one is delta log(avro). So when they want to re-ingest part of data, Hudi's handle logic will add these data into delta log, without modify the whole base file.

2. Iceberg
- Netflix's Iceberg is closest to the concept of metadata layers: they add a manifest at the top of all files, so they can know where the files store without list the whole directory. In this way we can have better control on file's list and improve performance.

3. Delta Lake
- Delta Lake was introduced by Databricks. They use the concept of "WAL (write ahead log)" on the data lake. Every time we change data will create a delta log (change log), and to record add/delete. Then every 10 files, these change log will be squashed into a base checkpoint file, to enhance IO performance.

### What They Resolved
- With metadata files ahead, user can not change low-level parquet files directly, but need to modify metadata record to ensure data is correct. In this way, we can put more strict rules and checks on metadata's writing process. For Example, all above three files format support concurrency write, but they will do write conflict check before accept it. And with the delta log, they can easily roll back to versions before writing. Besides, delta log can also ensure schema evolution, and time travel /version control of data. 

# ----------------- AI Generated ------------

## A Brief History of Data Systems
In the past, data systems were typically divided into two layers: a "Lake" layer for raw data and a "Warehouse" layer for cleaned, structured data. Today, the industry is rapidly shifting toward the "Lakehouse" pattern.

In the 1990s, when data teams were small, engineers often queried OLTP databases directly for reports. However, as businesses grew and integrated third-party sources (such as ad platforms and tracking systems), direct querying became difficult due to rate limits and resource contention. This led to the practice of scheduling nightly jobs to store data in shared databases for daytime processing.

By the 2000s, Apache Hadoop introduced object storage, allowing for the storage of everything from CSVs to media files. This "Data Lake" approach initially seemed ideal, but without organization, these lakes quickly turned into "Data Swamps." To fix this, engineers built complex pipelines to transform messy lake data into structured "Data Warehouses."

## The Emergence of the Lakehouse
In 2009, Meta released a paper on Apache Hive, an open-source query engine built on Hadoop. Hive allowed users to query structured data directly within the lake, eliminating the need to move data to a separate warehouse. This paved the way for tools like Impala, Trino, and Presto, all aimed at querying data faster while reducing storage and transfer costs.

In 2020, Databricks officially introduced the Data Lakehouse concept. They argued that the traditional two-layer structure was costly and difficult to maintain due to data redundancy and consistency issues. The Lakehouse aims to bridge this gap, though it faces challenges such as a lack of ACID compliance and poor data discovery. To resolve this, Databricks proposed a metadata layer between the lake and the warehouse to provide robust table abstractions.

## Critical Limitations of Apache Hive
- Lack of ACID Transactions: Unlike relational databases, Hive does not guarantee ACID properties; a network failure during transfer can easily lead to file corruption.

- Concurrency Issues: Hive lacks the locking mechanisms required to handle multiple users writing to the same table simultaneously.

- Metadata Bottlenecks: Hive must list all files in a directory before reading, which is time-consuming even when querying only a few partitions.

- Performance Hotspots: Hive stores data with similar prefixes together. In extreme cases, this causes all requests to hit a single server, creating performance bottlenecks.

# How Tech Giants Solved These Problems
Hudi (Uber): Created to handle incremental data refreshes. Instead of re-ingesting 120TB of data every 8 hours for a 1% change, Hudi splits data into base files (Parquet) and delta logs (Avro). This allows for targeted updates without modifying the entire base file.

Iceberg (Netflix): Closest to a true metadata layer. Iceberg uses a "manifest" to track file locations, enabling the system to find data without listing entire directories, significantly improving performance.

Delta Lake (Databricks): Implements "Write-Ahead Logs" (WAL) on the data lake. Every change creates a delta log (add/delete records), which is periodically squashed into a checkpoint file to maintain high I/O performance.

# Core Solutions Provided
By placing a metadata layer ahead of the raw storage, users no longer modify low-level Parquet files directly. This enables:

- Write Conflict Checks: All three formats support concurrent writes with strict validation.

- Data Governance: Features like Time Travel, version control, and Schema Evolution are now possible.

- Reliability: Delta logs allow for easy rollbacks and ensure data integrity through stricter writing processes.