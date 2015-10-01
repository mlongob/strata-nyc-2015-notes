# HDFS Erasure Coding
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
