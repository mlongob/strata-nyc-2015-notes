# Hive and Hbase for Transaction Processing
* Our goal:
    * Combine Hive, Hbase, Phoenix for single combine data store for analytics and transaction processing
* Brief History of Hive
    * Run on MapReduce, Tez, Spark
    * Since 0.11 ad-hoc queries
* LLAP: Why?
    * It's hard to be fast and flexible in Tez
        * Pre-allocate containers
    * No caching of data between queries
    * Live Long And Process (LLAP)
        * Node resident daemon process
        * Has in-memory columnar data cache
        * Uses YARN for resource management
        * LLAP is not MPP: Not a replacement for Tez or Spark
* Use hbase as back-end store for Hive Metastore
    * No need to normalize -&gt; Store directly
    * Cache much more aggressively
    * But:
        * Hbase does not support transaction
        * Hard to administer/install
        * Work we do on Hbase benefits all Hbase users
    * Hbase metastore
        * Not defaults. Users can still use RMDBS
        * Less than 10 tables in Hbase
        * Highly optimized for reads
        * Early performance test -&gt; about twice as fast
* Apache Phonix
    * SQL on top of Hbase
    * Oriented towards transactional processing
    * Working on more type operators
    * Working on adding transactions
    * Working to Apache Calcite for optimization
        * Same optimizer Hive uses
* What if?
    * We could share one O/JDBC driver?
    * We could share on SQL dialect?
    * Phoenix could leverage extensive analytics functionality in Hive without re-inventing it
    * Users could access their transactional and analytics data in single SQL operations?
* How?
    * LLAP is a storage plus operations for Hive. We can swap it out.
    * Tez and Spark can do post-shuffle operations with LLAP on Hbase
    * Calcite is built specifically to integrate disparate data storage systems
* Vision
    * User picks storage location for table in create table (LLAP or Hbase)
    * Transactions more efficient in Hbase tables but work in both
    * Analytics more efficient in LLAP tables but work in both
    * Queries that require shuffle use Tez or Spark for post shuffle operators
* Hurdles
    * Need to integrate types/data representation

