# Structural Index for Conjunctive Path Queries

We provide codebase for evaluating index techniques for conjunctive path queries.
You can use this code to easily evaluate state-of-the-art indexes for conjunctive path queries on edge-labeled graphs.
Additionally, we share datasets and workloads that we used experimental studies in WWW2021. We note that we cannot upload Freebase and Wikidata dataset due to its size.


## Setup

- CMake version up to 3.7

- [google dense hashing](https://github.com/sparsehash/sparsehash)

- We tested the codebase only on ubuntu 16.04. So, we are not sure whether other OS and other versions work well.

## Contents
The following are contents in this repository.
### constructing index
Our codebase provides four indexes: structural index, workload-aware structural index, path index, and workload-aware path index.

### query processing
We provide query processing algorithm with (workload-aware) structural and path indexes. Also, query processing without any indexes, which is based on bread-first-search.
Query processing with structural index also includes maintenance methods.

## Input Format
### Graph

The top of the line is triplet 'the number of vertices', 'the number of edges', and 'the number of labels'. Then, it lists edges which are triplets 'vertexId1' 'vertexId2' and 'labelID'.
The vertexIds must be within the range [0, # of vertices -1].
Here, we note that our codes automatically generate inverse of edges and labels, so the numbers of edges and labels must not include the inverse of edges and lagels, respectively.


The following is an example of graph data.
> 100 200 4
>
> 0 1 0
>
> 0 2 1
>
> ...

Please see a file in 'data' directory for a concrete example. The data is from [robots dataset](http://tinyurl.com/gnexfoy).

### Query
Our query is following conjunctive path queries, which have 'lookup', 'composition', 'conjunction', and 'identity'.
Since our codes do not have parsers, we need to give a query parsing tree by ourselves.

- lookup is represented by "a sequence of labelIds' (inverse label is 'labelId + # of labels')
- composition is represented by 'j'
- intersection is represented by 'c'
- identity is represented by three cases 'a sequence of labelIds id' (i.e., identity after look up), 'j_id' (i.e., identity after join) and 'c_id' (i.e., identity after conjunction)

Query '0->0->1->1' can be written as follows when structural or path index stores paths with 2 length.
> j
>
> 0 0
>
> 1 1

composition(identity(conjunction( (inverse of 0)->(inverse of 1)), (inverse of 2)->(inverse of 3)))), 2->3) can be written as follows when the number of labels are four and indexes store paths with 2 length.
> j
>
> c_id
>
> 4 5
>
> 6 7
>
> 2 3

Please see files in 'workload' directory for more examples.
Here, Query parser and optimization are a part of our future work.

### Workload
Workloads are the set of queries. Please add '!' at the end of queries.
Please see a file in 'workload' directory for more examples.

## Test
Run run.sh in each directory for testing each code.
