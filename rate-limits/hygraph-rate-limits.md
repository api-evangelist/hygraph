# Hygraph API Rate Limits

Hygraph enforces rate limits on uncached GraphQL operations. CDN-cached responses are not rate-limited.

## Requests Per Second (RPS)

Maximum new uncached API requests that can be initiated per second per environment.

| Plan       | Limit           |
|------------|-----------------|
| Hobby      | 5 req/sec       |
| Growth     | 25 req/sec      |
| Enterprise | Up to 500 req/sec |

**HTTP Error:** `429 Too Many Requests` when exceeded.

## Concurrent Operations

Maximum simultaneous uncached GraphQL operations per environment. This is measured independently from RPS — both limits must be respected.

| Plan       | Concurrent Queries | Concurrent Mutations |
|------------|--------------------|----------------------|
| Hobby      | 10                 | 5                    |
| Growth     | 30                 | 10                   |
| Enterprise | 60                 | 20                   |

**HTTP Error:** `429 Too Many Requests` when exceeded.

## Request Size Limits

Maximum byte size of a single GraphQL query or mutation body.

| Plan       | Query Limit | Mutation Limit |
|------------|-------------|----------------|
| Hobby      | 10 KB       | 30 KB          |
| Growth     | 15 KB       | 70 KB          |
| Enterprise | 20 KB       | 80 KB          |

**HTTP Error:** `413 Content Too Large` when exceeded.

## API Call Volume Limits

Monthly API operation caps per plan:

| Plan       | Monthly API Calls      |
|------------|------------------------|
| Hobby      | 500,000                |
| Growth     | 1,000,000              |
| Enterprise | 50,000,000+            |

- **Hobby:** Usage is blocked until the next billing period when the monthly limit is reached.
- **Growth:** Overages are charged at $0.20 per 1,000,000 additional API calls, billed at the end of the billing cycle.
- **Enterprise:** Custom limits negotiated with sales.

## CDN Cache Behavior

- Cached reads (CDN hits) are **not** rate-limited or size-restricted.
- The read-only cache endpoint delivers 3–5x lower latency.
- Maximizing cache hit rate is the recommended strategy for high-traffic use cases.

## Dedicated Clusters

Enterprise customers on dedicated clusters can have rate limits lifted or significantly increased. Contact Hygraph sales to discuss dedicated infrastructure options.

## References

- API Limits documentation: https://hygraph.com/docs/api-reference/basics/api-limits
- Rate limit blog post: https://hygraph.com/blog/hygraph-api-performance-rate-limits
