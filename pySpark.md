# pySpark


### Sorting and Ordering
`df.sort()` and `df.orderBy()` accomplish the same thing.

Ordering by a column:
* `df.orderBy("columnName", ascending=True)`
* `df.orderBy(F.col('columnName').asc())`
* `df.sort(F.col('columnName').asc())`

Ordering by multiple columns:
* `df.orderBy(F.col('columnName1').asc(), F.col('columnName2').desc())`
* `df.sort(F.col('columnName1').asc(), F.col('columnName2').desc())`

## Memory Management
`spark.catalog.clearCache()` - Removes all cached tables from the in-memory cache.

`df.unpersist()` - Mark this DataFrame as non-persistent, and remove all blocks for it from memory and disk.
