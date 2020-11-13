# ```coinmetrics-export```
```coinmetrics-export``` is a "tool used by CoinMetrics.io team for exporting data from blockchains into analytical databases" [[1]](#References).

In the case of Monero [[2]](#References), it retrieves data from a Monero full node via the following JSON RPC methods: ```get_block_count, get_block, get_transactions``` [[3]](#References).

We leverage the support in ```coinmetrics-export``` for Monero and PostgreSQL. 


## Table of contents
- [Commands](#commands)
  - [Continuous Export](#continuous-export)
  - [Print Schema](#print-schema)
- [Schema](#schema)
  - [Table ```monero```](#table-monero)
  - [Type ```MoneroBlock```](#type-moneroblock)
  - [Type ```MoneroTransaction```](#type-monerotransaction)
  - [Type ```MoneroTransactionInput```](#type-monerotransactioninput)
  - [Type ```MoneroTransactionOutput```](#type-monerotransactionoutput)
- [Materialized View ```monero_stats```](#materialized-view-monero_stats)
- [Build notes](#build-notes)
- [References](#references)



# Commands
## Continuous Export
Continuously export the Monero blockchain to a Postgres database.

### Command

```
coinmetrics-export export \
    --blockchain monero \
    --continue \
    --threads 1 \
    --output-postgres "host=127.0.0.1 port=5432 dbname=xmrdb user=*** password=***"
```


### Parameters

| Parameter | Description |
| - | - |
| ```--blockchain monero``` | Target Monero data files/output schema |
| ```--continue``` | On restart, query the last synchronized block in database and continue sync from there |
| ```--threads 1``` | "Number of threads talking to blockchain daemon; usually means increase in synchronization speed, but only up to some limit: test what number on your setup works better" [[4]](#References) |
| ```--output-postgres "pgsql connection string"``` | Output to PostgreSQL |


## Print Schema
Show the output schema.


### Command
```
coinmetrics-export print-schema --schema monero --storage postgres
```

### Parameters

| Parameter | Description |
| - | - |
| ```--schema monero``` | Monero schema |
| ```--storage postgres``` | PostgreSQL format |


### Output 
As of version Apr 18 2020 (commit 771feb7b654c5029eb8f3c0e7bf20787c8ba1c4c) [[5]](#References).

```
CREATE TYPE "MoneroTransactionInput" AS ("amount" BIGINT, "k_image" BYTEA, "key_offsets" BIGINT ARRAY, "height" BIGINT);
CREATE TYPE "MoneroTransactionOutput" AS ("amount" BIGINT, "key" BYTEA);
CREATE TYPE "MoneroTransaction" AS ("hash" BYTEA, "version" BIGINT, "unlock_time" NUMERIC, "vin" "MoneroTransactionInput" ARRAY, "vout" "MoneroTransactionOutput" ARRAY, "extra" BYTEA, "fee" BIGINT);
CREATE TYPE "MoneroBlock" AS ("height" BIGINT, "hash" BYTEA, "major_version" BIGINT, "minor_version" BIGINT, "difficulty" BIGINT, "reward" BIGINT, "timestamp" BIGINT, "nonce" BIGINT, "size" BIGINT, "miner_tx" "MoneroTransaction", "transactions" "MoneroTransaction" ARRAY);
CREATE TABLE "monero" OF "MoneroBlock" (PRIMARY KEY ("height"));
```



# Schema
## Table ```monero```
This is the Monero blockchain. It is structured as one table with custom data types.

- Type ```MoneroBlock```  
- Primary key: ```height```

Note: There is also an experimental "flat schema" mode, but it is "very experimental and very briefly tested" [[6]](#References).


## Type ```MoneroBlock```
| Column | Type |
| - | - |
| height | BIGINT |
| hash | BYTEA |
| major_version | BIGINT |
| minor_version | BIGINT |
| difficulty | BIGINT |
| reward | BIGINT |
| timestamp | BIGINT |
| nonce | BIGINT |
| size | BIGINT |
| miner_tx | MoneroTransaction |
| transactions | MoneroTransaction ARRAY |


## Type ```MoneroTransaction```
| Column | Type |
| - | - |
| hash | BYTEA |
| version | BIGINT |
| unlock_time | NUMERIC |
| vin | MoneroTransactionInput ARRAY |
| vout | MoneroTransactionOutput ARRAY |
| extra | BYTEA |
| fee | BIGINT |


## Type ```MoneroTransactionInput```
| Column | Type |
| - | - |
| amount | BIGINT |
| k_image | BYTEA |
| key_offsets | BIGINT ARRAY |
| height | BIGINT |


## Type ```MoneroTransactionOutput```
| Column | Type |
| - | - |
| amount | BIGINT |
| key | BYTEA |



# Materialized View ```monero_stats```
CoinMetrics.io provides one prewritten query based on this schema, located in a separate GitHub repository called "analytics" [[7]](#References).

Source: https://github.com/coinmetrics-io/analytics/blob/master/sql/monero_stats.sql


## Schema

| Column | Type | Description |
| - | - | - |
| date | DATE | Date; one record per day |
| block_cnt | BIGINT | Number of blocks |
| block_size | NUMERIC | Sum of block sizes |
| reward | NUMERIC | Sum of block rewards multiplied by ```1^-12``` |
| avg_difficulty | NUMERIC | Average of block difficulties |
| cnt | BIGINT | Number of transactions |
| fees | NUMERIC | Sum of transaction fees |
| med_fees | DOUBLE PRECISION | Median of transaction fees |
| io_cnt | NUMERIC | Number of inputs and outputs |
| payment_cnt | NUMERIC | Number of outputs |



# Build notes
The ```stack``` build command given in the README needed the following prerequisites:

- PostgreSQL (for ```pg_config```) [[8]](#References)
- ```libpq-dev```



# References
[1] GitHub - coinmetrics.io/haskell-tools: Tools for exporting blockchain data to analytical databases. https://github.com/coinmetrics-io/haskell-tools

[2] Monero - secure, private, untraceable. https://www.getmonero.org

[3] haskell-tools/coinmetrics-monero at master - coinmetrics-io/haskell-tools - GitHub. https://github.com/coinmetrics-io/haskell-tools/blob/master/coinmetrics-monero/CoinMetrics/Monero.hs

[4] haskell-tools/coinmetrics-export.md at master - coinmetrics-io/haskell-tools - GitHub. https://github.com/coinmetrics-io/haskell-tools/blob/master/docs/coinmetrics-export.md

[5] monero: change type of unlock_time field to Scientific - coinmetrics.io/haskell-tools@771feb7 - GitHub. https://github.com/coinmetrics-io/haskell-tools/commit/771feb7b654c5029eb8f3c0e7bf20787c8ba1c4c

[6] Support for flat schema - Issue #15 - coinmetrics-io/haskell-tools. https://github.com/coinmetrics-io/haskell-tools/issues/15

[7] GitHub - coinmetrics.io/analytics: Analytics scripts. https://github.com/coinmetrics-io/analytics

[8] PostgreSQL: The world's most advanced open source relational database. https://www.postgresql.org