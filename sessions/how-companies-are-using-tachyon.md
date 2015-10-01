# How companies are using Tachyon, a memory-centric distributed storage
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
