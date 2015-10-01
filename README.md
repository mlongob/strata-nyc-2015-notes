# Strata Hadoop NYC 2015
[Schedule](http://strataconf.com/big-data-conference-ny-2015/public/schedule/grid/public/)

## Keynotes
### The next generation - Kudu:
* [Kudu](http://blog.cloudera.com/blog/2015/09/kudu-new-apache-hadoop-storage-for-fast-analytics-on-fast-data/)
* Kudu fills the gap between HDFS and Hbase for live analytics (time series, high ingest)

### What 0-50 million users in 7 days can teach us about big data:
* Microsoft's Joseph Sirosh
* [How-Old.net](http://www.how-old.net)
* 50 million users in 7 days

## Breakout sessions
### What's coming for the Spark community:
* "Spark is the Taylor Swift of Big Data software"
* Feature releases every 3 months
* Incremental feedback-rich deployment
* Technical Directions
    * High level APIs
        * Spark API more expressive than Hadoop MapReduce
        * RDD Api is still not expressive enough -&gt; Data Frame API is better!
            * Give schema to the data (rows &amp; columns)
            * Leverage schema to simplify code and allow optimizations
            * Allows UDFs
            * Many built-in functions
            * Available in all supported driver languages
            * Better fit to relational problems (SQL)
        * Machine leaarning pipeline API
            * Simplify machine learning workflows
            * Allows under-the-hood optimizations
        * SparkR API for R
            * Suited for quants/data scientists
    * Performance
        * 91% of users are attracted to Spark because of performance
        * Project Tungsten
            * The CPU squeeze -&gt; Code generation
            * Garbage colleciton -&gt; External managed memory
            * Shift from JVM code to machine specific code (maybe through LLVM)
    * Pluggability and extensibility
        * Data sources API (Release Jan 2015)
            * Pulling/Writing data into/from Spark
            * Example: Json accessor (pass in schema), Parquet accessor (pass in append/overwrite)
            * Large number of IO libraries (built in and third-party)
        * Deployment integration
            * Spark on Mesos and YARN
                * Security intgration
                * Resource management
* Trends in the Spark community
    * Most Spark users in 2014 were using the core APIs -&gt; Now 51% are using 3+ libraries
    * Spark packages is a community service for third-party spark packages/libraries
        * More than 100 packages on Spark Packages
        * You can specify packages to be downloaded in spark_submit
        * Many data sources integrations
        * API extensions
    * Increasing storage options
        * Push down information to the storage accessor (example Cassandra API)
    * Deployment environments that include Spark
        * Distros: Cloudera, Hortonworks, MapR
        * Public clouds: Databricks, AWS, Google
* Learn more
    * SparkHub community site
    * Spark summit east
    * edX massive online courses (free)
    * Databricks training
    * O'Reilly books

### How companies are using Tachyon, a memory-centric distributed storage
* Open source
    * Started at UC Berkley AMPLab
    * Open Sourced in April 2013
    * Used by 100+ companies
    * 150+ contributors, 50 organizations
    * Default off-heap storage solution for Apache Spark
    * Used by Baidu in production
        * SparkSQL framework
        * 30x performance improvement
* Introduction to Tachyon
    * Memory is:
        * Fast
        * Cheaper
    * Issues
        * Data sharing is the bottleneck in analytics pipelines -&gt; Tachyon's memory speed solves the bottleneck
        * Cache loss when process crashes -&gt; Tachyon brings that out of process to keep the storage safe
        * Data duplication and garbage collection -&gt; no in-memory duplication, much less GC
    * Architecture
        * Memory centric storage architecture
            * Single Tachyon master
                * Has workflow manager
            * Multiple worker nodes
                * Worker daemon
                * Ramdisk (where the data is kept)
                * Readers will benefit from data locality
        * Push lineage
            * Can re-build data by keeping lineage information (similar to Spark)
* New features (0.8 release)
    * Eco-system
        * Integrate with other components: Spark, Hadoop, Hbase, OpenStack, HDSF, Ceph
    * Ability to run on-premise and in cloud-based deployments
    * Tiered storage (Higer capacity/Slower -&gt; Lower capacity/Faster)
        * Configurable tiers (Mem only, Mem/HDD, SSD only)
        * Promote hot data to upper tier
        * Evict stale data to lower tier
    * Transparent naming
        * Unified namespace
        * Sharing of data across storage systems (S3, Tachyon, HDFS)
        * API for on the fly mounting/unmounting
    * Remote write support
    * Easy deployment with Mesos/YARN
    * Inital security support
    * One-command cluster deployment
    * Metrics reporting for clients, workers, masters
    * More under storage support

### HDFS Erasure Coding
* Replication is expensive
    * Machine level replication works well (HDFS)
        * Storage overhead for 3x replication is 200%
    * XOR Coding
        * Half storage overhead
        * Slower recovery
    * Facebook F4 stores 65PB in Erasure Coding (EC)
        * Also Google and Amazon use it
* Background of Erasure Coding
    * Data durability -&gt; how many simultaneous failures can be tolerated?
    * Storage efficiency -&gt; how much portion of storage is useful data?
    * 3x Replication / NO EC
        * DD -&gt; 2
        * EC -&gt; 33%
    * XOR coding (with 6 data cells)
        * DD -&gt; 1
        * EC-&gt; 86%
    * Reed-Solomon (RS) coding with (6,3)
        * Another EC technique
        * DD -&gt; 3
        * SE -&gt; 67%
    * Reed-Solomon (RS) coding with (10, 4)
        * DD -&gt; 4
        * SE -&gt; 71%
    * EC in Distributed storage
        * Contiguous layout
            * Good for large files
            * Good for data locality
        * Striped layout
            * Good for small files
            * Good for parallel IO
            * Poor data locality
    * Choosing Block Layout
        * 3 clusters analyzed
            * Very few large files take up majority of space
            * Very few large files take up moderate amount of space
            * Mostly small files
            * Phase 1.1 -&gt; Striping &amp; EC
            * Phase 1.2 -&gt; Contiguous &amp; EC
    * Generalizing Block NameNode
        * Mapping Logical and storage blocks
            * Round robin assignment
        * Too many storage blocks? -&gt; Too much load on NameNode
        * Hierarchical naming protocol
            * NameNode only tracks logical node
                * First block is a single bit: EC or Replication -&gt; Allows hybrid deployment
        * Client parallel writing
            * Queue of packets assigns to a distributed streamer
                * Streamer is the consumer
                * Streamers can fail
        * Reconstruction on DataNode
            * Recover data on the background before it's requested
            * If parity data is lost, consequences are less severe
        * Hardware accelerated coders
            * Intel ISA-L
                * Legacy coder
                    * From Facebook HDFS-Raid project
                * 2 New coders
                    * Pure Java coder
                    * Native coder
                * Native code is 20x time faster than legacy coder
                    * Bottleneck is no longer in the calculation
                        * Back to disk IO
                        * In certain cases faster than 3-way replication
* Conclusion
    * EC expands space by ~50%
    * Just merged feature branch to apache Hadoop trunk!

### Hive and Hbase for Transaction Processing
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

### Real-world NoSQL schema design
* What does good mean (when it comes to schemas)?
    * Expressive
        * Must express the concepts we need
    * Efficient
        * Must run fast enough on cheap enough hardware
    * Instrospectable
        * Must be able to inspect the data and schema and gain understanding
        * Schema and data is easy to understand and explore
        * No name overflow
        * Scoping
* Relational theory is old
    * Older than Fortran
    * Pre-dates
        * Lexical scoping
        * Data structures
        * Functional programming
        * Logic programming
        * Recursive procedures
* Ojai is a MapR thing
* Tables as Objects / Objects as Tables
* If embedded tables are first class, schema becomes data
* Time series data
    * When columns are not pre-defined, they can convey information
    * Predefined schema is impossible for this idiom
    * Refactor relational-style time series schema into column oriented schema
        * Great performance
        * Great compression
* Music meta-data

### What's new in Spark Streaming - a technical overview
* Brief overview of how to use Kafka + Spark streaming for streaming word count
    * Need to initialize StreamingContext
    * Need to create DStream (Abstraction for streaming RDDs)
    * You can interactively query streaming data with Spark SQL
    * Many companies adopting it: Uber and Netflix among them
    * Recent survey
        * 850 organizations
        * 56% annual increase in usage
        * Hottest component of the spark ecosystem
* New features
    * Libraries
        * Streaming machine learning algorithms
            * True integration with ML-Lib
            * StreamingLinearRegression
            * StreamingKMeans
            * StreamingLogisticRegression
        * Python API Improvements
            * Added support for streaming machine learning
            * Kafka
            * Flume
            * Kinesis
            * MQTT
    * Ease of use
        * New visualizations in streaming UI
            * More graphs
            * More visibility of what's going on
                * Average processing time
                * Other batch-related statistics
                * Stability -&gt; Low scheduling delay
                * Want to compare processing time to batch time
                    * If p-time &gt; batch-time -&gt; Unstable system
            * List of active/past batches
                * Added list of offsets being processed by which batches
            * Full visual DAG for stages/RDD lineage analysis
                * Dig down in the abstraction stack Dstream -&gt; RDD
                * This is cool!
            * Memory usage statistics
                * Can catch issues with uneven memory distribution across executors
                * Good for debugging memory tuning
    * Infrastructure
        * Zero Data Loss Guarantee
            * Non-replayable sources
                * Example: Flume
                * Spark streaming now writes to own Write-Ahead-Log
                    * WAL is saved in fault-tolerant file system (like HDFS)
                    * Driver runs receivers
                    * Executors have receivers
                    * Receivers store WAL in HDFS
                    * In case of driver failures, checkpoints can be used to recover failed tasks
                    * Failed tasks will use the WAL to resume processing
                    * WAL is not enabled by default
                        * Enabled iwth spark.streaming.receiver.writeAheadLog.enable -&gt; true
                    * Should use reliable receivers
                    * Reliable receivers + WAL -&gt; No data loss guarantee
            * Replayable sources
                * Example: Kafka, Kinesis
                * Spark streaming saves only the record data and replays data directly from source
                * For Kinesis
                    * Only store sequence numbers in WAL
                        * Same WAL described in above section
                        * Can recover from failures to replay from Kinesis using the recovered sequence numbers
                        * More efficient than storing the whole data corpus
                        * Provides at-least-once guarantee
                * For Kafka
                    * A priori, decide what range of offsets is processed by a given batch
                        * Range is saved to HDFS for fault tolerance
                    * Direct Kafka API
                        * Does not use receivers
                        * No need for Spark Streaming to replicate
                        * Higher than 10x throughput than the receiver approach
                        * XXX Provides exactly-once guarantee
                        * Can run Spark batch jobs directly on Kafka
                            * RDD partitions = Kafka partition
        * System Stability
            * **Backpressure**
                * For stability data must be processed at least as fast as data is received
                * Since 1.1 -&gt; Allowed setting static limits ingestion rates on receivers to guard against spikes
                    * Ineffective for when downstream systems slow down
                * Backpressure is now introduced
                * Dynamically and automatically adapts rate to ensure stability under any processing conditions
                    * Uses processing time and scheduling delays to set rate limits
                    * PID controller theory is used to calculate rate limits
                * Experimental feature
                    * Disabled by default
                    * Will be enabled by default in following releases
* What's next?
    * Libraries and API
        * Support for event-time and out of order data
            * Use event-time in the record
                * Do window operations on it
                * Process out of order data
        * Tighter integration between Streaming and SQL+DataFrames
            * Helps leverage project Thungsten
                * Big sweeping ground breaking change in Spark Core engine
                * Manage memory in Spark instead of JVM
                * Higher performance
    * Infrastructure
        * Support for Dynamic Allocation for Streaming
            * Already supported for batch jobs
            * Dynamically manage resources at run time with YARN
            * Will work with backpressure to scale up/down while maintaining stability
            * As of 1.5 Dynamic allocation is not optimized for streaming
            * Users can build their own scaling logic using the developer API
        * Leverage project Thungsten
            * Improve performance of stateful operations
* Join us at Spark Summit Europe 2015 in Amsterdam

### Faster time to insight using Spark, Tachyon, and Zeppelin
* Stack
    * Spark + Spark streaming
    * Tachyon
    * On multiple fault tolerant storage solutions
    * Zeppelin for notebooks
* Using Databricks csv package to process CSV files
* Zepplin has the concept of interpreters
    * Spark interpreter
    * Shell interpreter
    * Hive interpreter
    * Cassandra interpreter
    * Interpreter API
* Data load
    * Use shell interpreter to load data into HDFS
    * Use pyspark interpreter to transform data in Parquet format
        * Scala API also supported
        * Parquet is supported as a core Spark accessor
        * He loaded parquet data into tachyon
* Data exploration
    * Used pyspark notebook to perform basic operations (count, print schema)
    * Used `sample` API in spark to sample the data corpus for exploration
    * Register the DataFrame to perform Spark SQL on it
    * The example has airline data
    * Spark SQL has it's own interpreter `%sql`
    * Zepplin has sharing (link to) capability
    * Ready-to-go charts are graphs built directly from the data in the notebook
* ETL
    * Used pypark notebook to do Spark RDD filters
    * Used Spark SQL UDFS for type conversion?
