---
layout: post
title: "database store engine base 1: LSM"
description: ""
category: 
tags: []
---
{% include JB/setup %}

##The Log-Structed Merge-Tree (LSM)
作为HBase存储的核心，有必要了解下
[pdf](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.44.2782&rep=rep1&type=pdf)
The Log-Structured Merge-tree (LSM-tree) is a disk-based data structure designed to provide low-cost indexing for a file experiencing a high rate of record inserts (and deletes) over an extended period.

disk-based 数据结构，对高频率的插入和删除提供低代价的索引

## 是什么？
A two component LSM-tree has a smaller component which is entirely memory resident, known as the C0 tree (or C0 component), and a larger component which is resident on disk, known as the C1 tree (or C1 component).
As each new History row is generated, a log record to recover this insert is first written to the sequential log file in the usual way. The index entry for the History row is then inserted into the memory resident C0 tree, after which it will in time migrate out to the C1 tree on disk; any search for an index entry will look first in C0 and then in C1. 

记录先写到log文件中，后写入c0（内存），之后再刷新到c1（磁盘，持久化）

从c0写入到c1时，是有延迟的，如果还没写入c1就挂机，此时需要通过log文件来进行恢复

c0 由于是内存，写入没有I/O cost，但是由于内存有大小限制，需要在达到一定条件（如达到某一大小）时，merge到c1中

The C1 tree has a comparable directory structure to a B-tree, but is optimized for sequential disk access, with nodes 100% full, and sequences of single-page nodes on each level below the root packed together in contiguous multi-page disk blocks for efficient arm use;
## 为什么效率高？
## 怎么实现
## 优缺点(相比B+树）
1. 优点：插入和删除操作很快(提升写的速度)
    The algorithm has greatly reduced disk arm movements compared to a traditional access methods such as B-trees, and will improve cost- performance in domains where disk arm costs for inserts with traditional access methods overwhelm storage media costs. The LSM-tree approach also generalizes to operations other than insert and delete.
2. 缺点：牺牲了读的速度（因为读的时候需要先merge）
    However, indexed finds requiring immediate response will lose I/O ef- ficiency in some cases, so the LSM-tree is most useful in applications where index inserts are more common than finds that retrieve the entries.

