# Faster time to insight using Spark, Tachyon, and Zeppelin
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
