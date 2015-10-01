# Real-world NoSQL schema design
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
