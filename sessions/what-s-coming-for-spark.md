# What's coming for the Spark community
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
