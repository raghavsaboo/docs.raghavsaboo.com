# Caching

Caching is a technique used in software systems to store frequently accessed or computationally expensive data in a temporary storage area known as a cache. By keeping this data readily accessible, caching aims to improve system performance and responsiveness.

![source: https://miro.medium.com/v2/resize:fit:1400/format:webp/1*jri7ODqZ2GlTC83vPTenxw.gif](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*jri7ODqZ2GlTC83vPTenxw.gif)

## Benefits of Caching

- **Faster Access**: Cached data can be retrieved more quickly than fetching it from its original source.
- **Reduced Latency**: Caching reduces the time required to access data, thereby decreasing latency for user requests.
- **Improved Scalability**: Caching helps alleviate the load on backend systems by serving cached data, enabling better scalability.
- **Bandwidth Conservation**: Caching reduces the amount of data transmitted over the network, conserving bandwidth.
- **Enhanced User Experience**: Faster response times lead to a better user experience, particularly for applications with high traffic volumes.

## Locations of Caching

- **Local/Colocated Cache**

  - Data is stored in memory on the same server/process as the application, providing fast access and low latency.
  - Suitable for scenarios where data is frequently accessed by a single application instance and does not need to be shared across multiple nodes.

- **Distributed Cache**

  - Caches are distributed across multiple nodes or servers to improve scalability and fault tolerance.
  - Data is replicated or partitioned across nodes, allowing for horizontal scaling and resilience to node failures.
  - Suitable for scenarios where data needs to be shared and accessed by multiple application instances or nodes in a distributed environment.

- **Client-side Caching**

  - Browser Caching: Web browsers cache resources like HTML, CSS, and JavaScript files to avoid redundant downloads and improve page load times.
  - Local Storage: Client-side storage mechanisms like localStorage and sessionStorage allow web applications to store data locally in the browser, reducing server requests and improving performance.

- **Web Server Caching**

  - Proxy Caching: Intermediate proxies or reverse proxies cache responses from backend servers, reducing server load and improving response times for subsequent requests.
  - Application-level Caching: Web servers and application frameworks provide mechanisms to cache dynamic content or database query results, reducing processing overhead and improving scalability.

- **Content Delivery Network (CDN) Caching**

  - Static or cacheable content is cached on edge servers distributed geographically to reduce latency for users worldwide.
  - CDN caches commonly include static assets like images, videos, and documents, improving their delivery speed and reducing server load.

- **Database Caching**
  - Queries and query results are cached to reduce the load on the database and improve query response times.
  - Database caching can be implemented at various levels, including query result caching, object caching, or query plan caching, depending on the database system and application requirements.

## Terminology

- **Cache Hit**:

  - A cache hit occurs when a requested item is found in the cache, resulting in a faster response time as the data can be retrieved directly from the cache.

- **Cache Miss**:

  - A cache miss occurs when a requested item is not found in the cache, requiring the data to be fetched from its original source, resulting in a longer response time.

- **Cache Hit Ratio (Hit Rate)**:

  - The cache hit ratio, or hit rate, is the ratio of cache hits to the total number of cache accesses (hits plus misses). It indicates the effectiveness of the cache in serving requests and is often expressed as a percentage.

- **Cache Invalidation**:

  - Cache invalidation is the process of removing or updating cached items when the underlying data changes, ensuring the cache remains consistent with the source of truth. This prevents stale or outdated data from being served to users.

- **TTL (Time to Live)**:

  - TTL, or Time to Live, is a parameter that specifies the duration for which cached data remains valid before it is considered stale and needs to be refreshed from the original source. It is commonly used in cache systems to control the lifespan of cached items.

- **Cache Coherence (Cache Consistency)**:

  - Cache coherence, or cache consistency, refers to the property of ensuring that multiple caches holding copies of the same data remain synchronized and consistent with each other. This ensures that all cache copies reflect the most recent updates to the data.

- **Cache Replacement Policy**:

  - Cache replacement policy defines the strategy used to select which items in the cache should be evicted when the cache reaches its capacity and needs to make space for new entries. Common replacement policies include Least Recently Used (LRU), First-In-First-Out (FIFO), and Random Replacement.

- **Cache Write Policy**:

  - Cache write policy determines how data modifications are handled in the cache. Common write policies include Write-Through, where data is written to both the cache and the underlying storage simultaneously, and Write-Back, where data is initially written only to the cache and later flushed to the underlying storage.

- **Cache Read Policy**:
  - Cache read policy determines how read requests are handled in the cache. Common read policies include Read-Through, where read requests are served directly from the cache if the data is available, and Read-Ahead, where the cache anticipates future read requests and pre-fetches data into the cache before it is requested.

## Best Practices

- **Identify Hotspots**: Determine which data should be cached based on access patterns and usage frequency.
- **Monitor Cache Performance**: Regularly monitor cache hit rates, miss rates, and eviction rates to optimize cache configuration.
- **Consider Cache Expiration**: Set appropriate expiration times for cached data to balance freshness with performance.
- **Plan for Cache Warming**: Pre-load the cache with frequently accessed data during system startup or maintenance periods to minimize cache misses.

## Cache Replacement Policies

| Policy                      | Description                                                                                                                         | Pros                                                                                       | Cons                                                                                                                    |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| Least Recently Used (LRU)   | The least recently used items are evicted from the cache when it reaches its capacity.                                              | Effective in removing items that are least likely to be used in the near future.           | Requires tracking of access times for cache items, which may introduce overhead.                                        |
| Least Frequently Used (LFU) | The least frequently used items are evicted from the cache when it reaches its capacity.                                            | Removes items that have been accessed the least number of times, reducing cache pollution. | Requires maintaining access frequency counts for cache items, which may consume additional memory and processing power. |
| Most Recently Used (MRU)    | The most recently used items are retained in the cache, and the least recently used items are evicted when it reaches its capacity. | Ensures that recently accessed items remain in the cache, improving cache hit rates.       | May lead to cache pollution if frequently accessed items are not representative of future access patterns.              |
| First-In-First-Out (FIFO)   | The oldest items are evicted from the cache first when it reaches its capacity.                                                     | Simple and straightforward eviction strategy.                                              | May not reflect the actual access patterns of cache items.                                                              |
| Last-In-First-Out (LIFO)    | The most recently added items are evicted from the cache first when it reaches its capacity.                                        | Reflects the order in which items were added to the cache.                                 | May not be suitable for scenarios where recently added items are more likely to be accessed again.                      |
| Random Replacement          | Randomly selects items from the cache for eviction when it reaches its capacity.                                                    | Simple to implement and does not require tracking access patterns.                         | May result in suboptimal cache performance if important or frequently accessed items are evicted randomly.              |

### Choosing the Right Replacement Policy

LRU and LFU are the most commonly used.

- **Considerations**:
  - **Maximizing Hit Ratio**: Select a cache replacement policy that maximizes the hit ratio by evicting the least useful items from the cache.
  - **Data Update Frequency**: Choose a policy that aligns with the frequency of data updates in the application.
  - **Consistency Requirements**: Determine the level of consistency required between cached data and the source of truth.
  - **Performance Overhead**: Evaluate the overhead introduced by different invalidation mechanisms on system performance and scalability.
- **Combination**: In many cases, a combination of invalidation policies may be used to address different requirements for different data sets within the application.

### LRU Code Example

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = OrderedDict()

    def get(self, key):
        if key in self.cache:
            # Move the accessed key to the end to indicate it's the most recently used
            self.cache.move_to_end(key)
            return self.cache[key]
        return -1

    def put(self, key, value):
        if key in self.cache:
            # If key already exists, update its value and move it to the end
            self.cache[key] = value
            self.cache.move_to_end(key)
        else:
            # Check if cache is full
            if len(self.cache) >= self.capacity:
                # If cache is full, remove the least recently used item (first item)
                self.cache.popitem(last=False)
            # Add the new key-value pair to the cache
            self.cache[key] = value

# Example usage
cache = LRUCache(2)  # Create a cache with capacity 2
cache.put(1, 1)
cache.put(2, 2)
print(cache.get(1))  # Output: 1
cache.put(3, 3)  # Evicts key 2, as key 1 was accessed most recently
print(cache.get(2))  # Output: -1 (Key 2 is no longer in the cache)
cache.put(4, 4)  # Evicts key 1
print(cache.get(1))  # Output: -1 (Key 1 is no longer in the cache)
print(cache.get(3))  # Output: 3
print(cache.get(4))  # Output: 4
```

## Cache Read Policies

1. **Read-Through**:

   - With this policy, when a read request is made for a data item, the cache first checks if the item is present. If the item is found in the cache, it is returned to the requester. If the item is not found, the cache fetches the data from the underlying data source (such as a database), stores it in the cache, and then returns it to the requester.

2. **Read-Ahead**:

   - Read-ahead, also known as prefetching, involves anticipating future read requests and proactively fetching data items into the cache before they are requested. This policy aims to reduce latency by preloading data items into the cache in anticipation of future access.

3. **Cache-Aside**:
   - In cache-aside (also known as lazy loading), the application code is responsible for checking the cache for a requested item before accessing the underlying data source. If the item is found in the cache, it is returned to the requester. If not, the data is fetched from the underlying data source, stored in the cache, and then returned to the requester.

## Cache Write Policies

1. **Write-Through**:

   - Data is written to both the cache and the underlying storage simultaneously. Acknowledgments are sent only after both the cache and the storage have been updated.

2. **Write-Back**:

   - Data modifications are initially written only to the cache and later asynchronously flushed to the underlying storage. Acknowledgments are sent once data is written to the cache, with flushing to the storage happening in the background.

3. **Write-Around**:
   - Data modifications are written directly to the underlying storage, bypassing the cache. Acknowledgments are sent only after data is successfully written to the storage. Data is loaded into the cache on a read cache miss.

### Tradeoffs

| Write Policy  | Advantages                                                                 | Disadvantages                                                            | Best Use Cases                                                               |
| ------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| Write-Through | - Strong consistency between cache and storage.                            | - May impact availability due to synchronous writes to storage.          | - Applications requiring strong data consistency.                            |
|               | - Guarantees data durability since data is written to storage immediately. |                                                                          | - Transactional systems and applications where data written is re-read immediately and frequently.                                                    |
|               | - Minimizes risk of data loss in case of cache failure.                    |                                                                          |                                                                              |
| Write-Back    | - Improved write performance due to asynchronous writes to storage.        | - Potential inconsistency until data is flushed to storage.              | - Applications with high write throughput that can accept data inconsistency or loss in order to optimize latency. |
|               | - Enhances cache availability with immediate acknowledgment.               | - Increased risk of data loss if cache fails before flushing to storage. | - Caching systems prioritizing performance over strong consistency.          |
|               | - Reduces I/O latency by delaying writes to storage.                       |                                                                          |                                                                              |
| Write-Around  | - Reduces cache pollution for infrequently accessed data.                  | - Eventual consistency; cache may not always have the latest data.       | - Archival or logging systems where data is written once and rarely read.    |
|               | - Minimizes cache overhead for data with unpredictable access patterns.    | - Increased read latency for recently written data not yet in cache.     | - Applications prioritizing write performance over read performance.         |
|               | - Prevents cache from filling with data that will not be re-accessed.      |                                                                          |                                                                              |
