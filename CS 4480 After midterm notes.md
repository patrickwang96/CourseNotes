# CS 4480 After midterm notes

## Topic 3 Spark

_In memory Large-scale data processing_   


* Spark
    * Resilient Distributed Memory Dataset(RDD)
    * Scala/Python/Java -- functional programming

* Spark SQL 
    * SchemaRDD

    
------

* In-memory Processing -> fast but volatility 
* Spark: 
    1. fault-tolerant
    2. efficiant

* RDD
    * Special built for data-intensive:
        * immutable , partitioned collections of records
        * no rapid updates on small memory blocks.
    * Fault recovery using linage

* Scala -> high-level functional, object-oriented language running on JVM
* **Map and flatmap difference**


--------

* Spark SQL
    * One of the Spark components
        * Executed SQL on Spark
        * Builds SchemaRDD 
    * Referring the Schema using Reflection 

* GraphX
    * Resilient Distributed Property Graph (RDG)

    
## Topic 4 Key-Value Stores

### BigTable and HBase

A table in BigTable is a sparse, distributed, persistent multidimensional sorted map.   

Map indexed by a row-key, column-key, and a timestamp.   

* Rows and Columns
    * Rows maintained in sorted lexicographic order. 
    * Columns grouped into column families. 
* Chubby: Lock Service for Loosely-Coupled Distributed System.   
* SSTable: Sorted String Table storing a set of immutable row fragments in sorted order based on row keys


#### SSTable
* Persistent, ordered immutable map from keys to values
    * stored in GFS
* Sequence of blocks on disk plus an index for block lookup
    * can be completely mapped into memory
* Supported operations:
    * Look up value associated with key
    * Iterate key/value pairs within a key range
        
#### Tablet
* Dynamically partitioned range of rows
* Built from multiple SSTables
#### Table
* Multiple tablets make up the table
* SSTable can be shared 

### Architecture
* Client library
* Single master server
* Tablet servers

#### Bigtable Master
* assigns tablets to tablet servers
* Detects addition and expiration of tablet servers
* Balances tablet server load
* Handles garbage collection
* Handles schema changes

#### Bigtable Tablet Servers
* Each tablet server manages a set of tablets
    * Typically between ten to a thousand tablets
    * Each 100-200 MB by default
* Handles read and write requests to the tablets
* Splits tablets that have grown too large

#### Tablet Assignment
* Master keeps track of:
    * Set of live tablet servers
    * Assignment of tablets to tablet servers
    * Unassigned tablets
* Each tablet is assigned to one tablet server at a time
    * Tablet server maintains an exclusive lock on a file in Chubby
    * Master monitors tablet servers and handles assignment
* Changes to tablet structure
    * Table creation/deletion (Master initiated)
    * Tablet merging (master initiated)
    * Tablet splitting (tablet server inititated)
#### Compactions 
    * Minor compaction
        * Converts the memtable into an SSTable
        * Reduces memory usage and log traffic on restart
    * Merging compaction
        * Reads the contents of a few SSTables and the memtable, and writes out a new SStable
        * Reduces number of SSTables
    * Major compaction
        * Merging compaction that results in only one SSTable
        * No deletion records, only live data
        * 

