# What's new in Spark Streaming - a technical overview
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
