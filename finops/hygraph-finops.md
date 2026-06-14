# Hygraph FinOps

Financial operations guidance for managing Hygraph spend, understanding cost drivers, and optimizing usage across plans.

## Cost Structure

Hygraph pricing is based on a combination of subscription tier and metered overages. The primary cost drivers are:

- **Subscription plan** — fixed monthly fee per tier (Hobby: $0, Growth: $199/month, Enterprise: custom)
- **API Operations** — total count of uncached GraphQL API calls per billing period
- **Asset Traffic** — outbound bandwidth consumed by asset delivery

## Overage Charges (Growth Plan)

| Resource        | Overage Rate                     |
|-----------------|----------------------------------|
| API Operations  | $0.20 per 1,000,000 calls        |
| Asset Traffic   | $0.20 per GB                     |

Overages are calculated at the end of each billing cycle and charged automatically to the payment method on file.

## Cost Optimization Strategies

### Maximize CDN Cache Hit Rate

CDN-cached responses are not counted against API operation quotas and are not rate-limited. Design content delivery queries to target published content via the read-only cache endpoint to reduce billable API calls.

### Use the Read-Only Cache Endpoint

Route all content delivery traffic through the CDN cache endpoint rather than the live API endpoint. Only use the live endpoint for authenticated mutations and draft previews.

### Batch Queries

GraphQL allows fetching multiple content types in a single request. Consolidate multiple separate requests into batched queries to reduce total API operation count.

### Optimize Asset Delivery

- Compress and resize assets before uploading to reduce storage and delivery bandwidth.
- Use Hygraph's built-in image transformation parameters (width, height, quality) to serve right-sized images rather than full-resolution originals.
- Consider using a dedicated CDN or image optimization service in front of Hygraph asset URLs for high-traffic production deployments.

### Monitor Usage in the Dashboard

Hygraph's project dashboard surfaces current-period API call counts and asset traffic. Review usage before billing cycle end to anticipate overages.

### Environment Hygiene

Growth plan includes 2 environments; Enterprise includes up to 10. Avoid running unnecessary staging environments that generate API traffic and inflate operation counts.

### Evaluate Plan Fit

- Teams consistently exceeding Growth plan limits at overage pricing should evaluate whether Enterprise plan pricing (negotiated flat rate) would be more cost-effective.
- Teams on Hobby who are consistently blocked at 500K calls/month should upgrade to Growth to avoid disruption.

## Budget Estimation

Estimate monthly cost for Growth plan customers:

```
Monthly Cost = $199 + (API calls overage × $0.20/1M) + (asset traffic overage × $0.20/GB)
```

**Example:** 3M API calls/month + 50 GB asset traffic overage
```
$199 + (2 × $0.20) + (50 × $0.20) = $199 + $0.40 + $10.00 = $209.40/month
```

## Enterprise Considerations

Enterprise contracts typically include:

- Negotiated flat-rate pricing replacing metered overages
- Dedicated cluster infrastructure removing shared rate limits
- Custom SLA with up to 99.95% uptime guarantee
- Volume discounts for multi-year commitments
- Bank transfer payment options for procurement compliance

## References

- Pricing page: https://hygraph.com/pricing
- API Limits: https://hygraph.com/docs/api-reference/basics/api-limits
