---
layout: post
title: When Data Grows
author: zhangyue
---
When Data Grows

* Availability
    * Write Availability
        * Quorum Replicaiton
        * CRDT (Conflict Free Data Type)
        * Erasure Coding
            * Reed solomon
    * Query Availability
* Consistency
    * Leader based
        * with vector clock lease
        * with wall clock lease
            * Mainstream Master-slave
            * Raft
    * Log based, multi-master
        * Paxos
        * Corfu
* Write Speed
    * Parallel computing
        * Multi node
        * Multi core
        * SIMD
            * SIMD based compression
    * Access pattern based optimization
        * Sequential write is faster than random write
        * Batch writing, group random write into sequential write
        * Write ahead logging
            * log structured data storage
    * Media based optimization
        * L1 > L2 > L3 > Memory > SSD > Spin Disk
        * In memory buffering
            * light weight compression
* Query Speed
    * Parallel computing
        * Scatter-Gather
        * Multi node
        * Multi core
        * SIMD / GPU
            * SIMD based decompression
                * Number codec
                    * Frame of reference
                * String codec
                    * Dictionary encoding
            * SIMD based search
                * linear search
                * binary search
            * SIMD based group by & projection
                * AMD APU & Intel IGP with 64gb Main Memory Access
            * SIMD based join
        * Bit level parallism
    * Scan less data by skipping
        * Locality
            * Data within one node
            * Data within one file
        * Sorting based index
            * B-tree
                * mysql innodb
            * LSM-tree
                * rocksdb
                * leveldb
            * Skip-list
            * Trie / FST
                * lucene
        * Hashing based index
            * bitcask
            * bloom filter
                * bitfunnel
    * Access pattern based optimization
        * Sequential read is faster than random read
        * Columnar layout
    * Media based optimization
        * L1 > GPU Memory > L2 > L3 > Main Memory > SSD > Spin Disk
        * In GPU Memory database
        * In Main Memory database
            * MMap


