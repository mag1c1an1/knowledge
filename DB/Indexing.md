# Indexing

- **Ordered indices**: Based on a sorted ordering of the values.
- **Hash indices**: Based on a uniform distribution of values across a range of buckets.  The bucket to which a value is assigned is determined by a function, called a _hash function_.

Factors:
- Access type
- Access time
- Insertion time
- Deletion time
- Space overhead

Search Key: An attribute or set of attributes used to look up records in a file.


## Ordered Indices
clustering index (primary indices): An index whose search key also defines the sequential order of the file.

nonclustering index (secondary indices): Indices whose search key specifies an order different from the sequential order of the file.

Dense index  
In a dense index, an index entry appears for every search-key value in the file. In a dense clustering index, the index record contains the search-key value and a pointer to the first data record with that search-key value. The rest of the records with the same search-key value would be stored sequentially after the first record, since, because the index is a clustering one, records are sorted on the same search key.  In a dense nonclustering index, the index must store a list of pointers to all records with the same search-key value.

Sparse index  
In a sparse index, an index entry appears for only some of the search key values. Sparse indices can be used only if the relation is stored in sorted order of the search key; that is, if the index is a clustering index. As is true in dense indices, each index entry contains a search-key value and a pointer to the first data record with that search-key value. To locate a record, we find the index entry with the largest search-key value that is less than or equal to the search-key value for which we are looking. We start at the record pointed to by that index entry and follow the pointers in the file until we find the desired record.

Multilevel Indices  
You know what it is.

### Insertion
- Dense indices:  
  1. If the search-key value does not appear in the index, the system inserts an index entry with the search-key value in the index at the appropriate position.
  2. Otherwise the following actions are taken:    
    &nbsp;&nbsp;a. If the index entry stores pointers to all records with the same search-key value, the system adds a pointer to the new record in the index entry.  
    &nbsp;&nbsp;b. Otherwise, the index entry stores a pointer to only the first record with the search-key value. The system then places the record being inserted after the other records with the same search-key values.
- Sparse indices:  
  We assume that the index stores an entry for each block. If the system creates a new block, it inserts the first search-key value (in search-key order) appearing in the new block into the index. On the other hand, if the new record has the least search-key value in its block, the system updates the index entry pointing to the block; if not, the system makes no change to the index.

### Deletion
- Dense indices:  
  1. If the deleted record was the only record with its particular search-key value, then the system deletes the corresponding index entry from the index.
  2. Otherwise the following actions are taken:   
    &nbsp;&nbsp;a. If the index entry stores pointers to all records with the same search-key value, the system deletes the pointer to the deleted record from the index entry.  
    &nbsp;&nbsp;b. Otherwise, the index entry stores a pointer to only the first record with the search-key value. In this case, if the deleted record was the first record with the search-key value, the system updates the index entry to point to the next record.
- Sparse indices:
  1. If the index does not contain an index entry with the search-key value of the deleted record, nothing needs to be done to the index.
  2. Otherwise the system takes the following actions:  
  &nbsp;&nbsp;a. If the deleted record was the only record with its search key, the system replaces the corresponding index record with an index record for the next search-key value (in search-key order). If the next search-key value already has an index entry, the entry is deleted instead of being replaced.  
  &nbsp;&nbsp;b. Otherwise, if the index entry for the search-key value points to the record being deleted, the system updates the index entry to point to the next record with the same search-key value.

Secondary indices must be dense, with an index entry for every search-key value, and a pointer to every record in the file.

composite search key: A search key containing more than one attribute.

## B+ Tree Index Files