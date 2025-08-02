
# 9.4 Duplicate URLs: You have 10 billion URLs. How do you detect the duplicate documents? In this case, assume "duplicate" means that the URLs are identical.

## Problem
You have 1 billion URLs, and you want to deduplicate them efficiently.

## Constraints
- Cannot fit all data in memory
- May contain duplicate URLs, possibly over 1000 times
- Must handle cases where all duplicates go to the same location
- Must deduplicate across all data

## Key Idea
Use **hashing** to partition the URLs into smaller buckets (files) that can be processed in-memory.

## Approach
1. **Hash each URL** to determine its bucket (e.g., 1000 buckets).
   ```
   int index = url.hashCode() % 1000;
   writeTo("bucket_" + index + ".txt", url);
   ```
   → Same URL always ends up in the same file.

2. **Deduplicate each bucket**
   - Load each file into memory
   - Use `Set<String>` to remove duplicates
   - Write deduplicated results to new file `unique_bucket_###.txt`

3. **Combine the unique results**
   - Merge all `unique_bucket_###.txt` into one deduplicated list

## Notes
- **Hash collisions** are okay: identical hashes → same bucket → deduplicated within
- File I/O speed is important; parallelize reading/writing when possible
- Can be extended with MapReduce (e.g., Hadoop, Spark)

## Scaling
- For larger data sizes: use distributed systems like **Hadoop**, **Spark**
- For smaller scale: use local disk and batch processing

