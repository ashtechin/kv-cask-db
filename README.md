## Cask DB is a database based on BitCask, a Log-Structured Hash Table for Fast Key/Value Data.
Note: I have tried this db on concept to learn about DBs and their implementations while reading "Designing data intensive applications". This is a leaderless DB, running on a single machine, so didn't have to worry about syncing and consensus. Will delve further into those topics later and try out some optimizations if I get some new ideas.
### It has following properties
1. Fast read and writes.
2. High throughput
3. Easy crash handling and restore because of write ahead logs
4. Simple implementation
5. Both in memory and on disk storage

### Some cons it has:
1. No proper deletion, it treats deletion as a overwrite. So a lot of high disk storage if frequent deletions.
2. No range queries (will soon try if possible)
3. High RAM usage because we are keeping key-values map in memory
4. Takes time to boot because the above key value map is loaded in memory on startup.

### How it works
We keep data in 2 places, in memory and on disk.

On disk: We will append a block to the active file, which contains key and value things in bytes. This will contain timestamp to make it serializable and get consistent writes. 

In memory: We will keep a map, with key and other metadatas like file name, value size, offset etc.

Suppose if we want to get a value corresponding to a key. We will go in memory and check out this map. This will give us an idea of where the value is, ie which file and at what position. Then we go to that location and fetch that value till the offset. This process is very fast because we don't need to scan the entire file on disk.

In case of inserting a value, we will just append it at the end of current file and store the corresponding metadata to the map in RAM.

Deletion and write are the same, which is basically instead of overwriting the value, creating a new file entry instead and mapping the key in memory to the new value. This might lead to a lot of storage waste, but gives fast and consistent writes.