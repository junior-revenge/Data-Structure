
# 8.1 Stock Data

## The Question
Stock Data: Imagine you are building some sort of service that will be called by up to 1,000 client
applications to get simple end-of-day stock price information (open, close, high, low). You may
assume that you already have the data, and you can store it in any format you wish. How would you
design the client-facing service that provides the information to client applications? You are responsible
for the development, rollout, and ongoing monitoring and maintenance of the feed. Describe
the different methods you considered and why you would recommend your approach. Your service
can use any technologies you wish, and can distribute the information to the client applications in
any mechanism you choose.

## Questions I Will Ask Before Going into Solution
- What business problem are we solving (internal use, external customers, compliance, …)?
    This question is crucial since the problem we establish will determine the focus and aim of the design.
- Who are the primary client types (mobile apps, web front-ends, backend batch jobs)?
    This question is also crucial in that this will determine which tech stack is more appropriate and effective.
- Any GraphQL or gRPC preference, or should we stick to REST?
    This question is important for structuring how client and server will communicate.
- Which KPIs matter most (cache hit ratio, publish lag, error rate)?
    Answer to this question will decide priorities when choosing tech stack.
- Some traffic rate related questions would be:
    Expected QPS peak and average? (Now and 12 months out.)
    Expected payload sizes per call?
    Target latency (P95/P99) for cache hits and misses?
    Do we expect multi-region client traffic?
    What cache TTLs are acceptable? Do we need ETag / If-Modified-Since semantics?
- Some less important business related questions would be:
    How much history must we expose (months, years, full)?
    How do we define “EOD” for multi‑timezone, multi‑exchange universes?
    Max number of tickers per request? Pagination rules?

## Overall Approach

1) Requirements & assumptions (explicitly stated)
    Data shape: {symbol, date, open, high, low, close, volume, adjusted_close?}.
    Freshness: once per trading day (EOD). Near‑real‑time is not required.
    Access pattern: heavy reads, tiny / no writes from clients.
    Clients: heterogeneous (mobile, web, backend cron jobs).
    Scale: 1,000 clients → modest. We can optimize for simplicity + cacheability.
    Immutability: once published, EOD rows are effectively immutable (perfect for CDNs & long cache TTLs).
    Global distribution: desirable but not mandatory.
    You own dev, rollout, monitoring, and maintenance.

2) Deliver Methods
    REST + CDN + Redis: Simple, universal, easy to scale, and widely popular so easy to get resources and develop reasonably faster.
```python
          ┌───────────────┐
 (Vendors/ETL) →  EOD ETL  │
          └──────┬────────┘
                 │ (batch write)
        ┌────────▼──────────┐
        │  OLTP DB (Postgres│/ClickHouse)  ← canonical store
        └──────┬────────────┘
        (materialize + export)
           ┌────▼────┐        ┌───────────────┐
           │ Redis   │        │  S3 / GCS      │  (bulk CSV/Parquet/JSON)
           └────┬────┘        └──────┬────────┘
                │                    │
         ┌──────▼─────────┐          │
         │  REST API      │◀─────────┘ pre-signed URLs (optional)
         │  (FastAPI/Go)  │
         └──────┬─────────┘
                │
           ┌────▼──────┐
           │ CDN/Edge  │  (CloudFront/Fastly)
           └────┬──────┘
                │
         1,000 client apps

```

3) Data model & storage

    Postgres (simple, ACID, sufficient scale) or ClickHouse (columnar, blazingly fast for historical scans).
    ```sql
    CREATE TABLE eod_prices (
    symbol TEXT NOT NULL,
    date   DATE NOT NULL,
    open   NUMERIC(12,4),
    high   NUMERIC(12,4),
    low    NUMERIC(12,4),
    close  NUMERIC(12,4),
    volume BIGINT,
    adj_close NUMERIC(12,4),
    PRIMARY KEY(symbol, date)
    );
    ```
    Bulk analytics: daily Parquet (columnar, compressed) → S3 + CloudFront.
    Cache: Redis for hottest keys (e.g., “latest” day, top 100 symbols).

4) Auth, quotas, and governance
    API keys (simple) or JWT (scoped; easier to roll per-client rates/scopes).
    Rate limiting: Per-key, e.g., 60 req/min default, burst 120.

5) Rollout strategy
    Blue/green or canary deploys behind a feature flag (e.g., LaunchDarkly) or at the API Gateway.

8) Service Level Objectives

    Availability ≥ 99.9%.

    P95 latency (cache hit) < 50 ms, (origin) < 200 ms.

    EOD publish deadline (e.g., by 18:00 local exchange time).

