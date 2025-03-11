# Cask DB is a database based on BitCask, a Log-Structured Hash Table for Fast Key/Value Data.
Note: I have tried this db on concept to learn about DBs and their implementations while reading "Designing data intensive applications". This is a leaderless DB, running on a single machine, so didn't have to worry about syncing and consensus. Will delve further into those topics later and try out some optimizations if I get some new ideas.
### It has following properties
1. Fast read and writes.
2. High throughput
3. Easy crash handling and restore because of write ahead logs
4. Simple implementation
5. Both in memory and on disk storage

### Some cons it has:
1. No proper deletion, it treats deletion as a overwrite. So a lot of high disk storage if frequent deletions.
2. No range queries
3. High RAM usage because we are keeping key-values map in memory
4. Takes time to boot because the above key value map is loaded in memory on startup.