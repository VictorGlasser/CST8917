# CST8917 Lab 1
## Research Azure Database Options
### Azure cosmos DB
Cosmos DB is a NoSQL relational database and supports "Key/value (table), columnar, document, and graph data models" natively. (https://docs.azure.cn/en-us/cosmos-db/faq). CosmodDB's serverless account type charges users for they use.

### Azure Table Storage
Azure Table Storage is a key-value store. This is in contrast to Cosmos DB, which is a NoSQL relational database. Queries can be much more specific in cosmosDB since, for example, the wordCount can be retrieved from the JSON as opposed to having to return the entire JSON with an Azure Table Storage since you only get the key-value pair. It's appropriate for "scenarios like storing user session data, metadata, and simple logs." (https://build5nines.com/how-to-choose-between-azure-cosmos-db-for-table-and-azure-table-storage/). This lacks scalability if JSONs become more complex.

### Azure SQL Database
Azure SQL database is a managed database service offered by Azure for relational databases with a fixed schema. SQL is used over NoSQL "for applications with complex queries and transactions" since it supports complex joins. (https://www.geeksforgeeks.org/sql/sql-vs-nosql-which-one-is-better-to-use/)

### Azure Blob Storage (article found using AI)
Can it be used as a database? What are the trade-offs?
Yes, Azure Blob Storage can be used as a database, but only to a degree. The pro is that it's very cheap, but the cons are that it isn't queryable or transactional. 
(https://blog.devgenius.io/azure-blob-storage-is-not-a-database-heres-the-microsoft-approved-fix-c51278ce3bf9)
