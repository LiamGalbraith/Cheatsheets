# pySpark

### Memory Management
`spark.catalog.clearCache()` - Removes all cached tables from the in-memory cache.

`df.unpersist()` - Mark this DataFrame as non-persistent, and remove all blocks for it from memory and disk.
