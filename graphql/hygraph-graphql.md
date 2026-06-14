# Hygraph GraphQL Content API

## Overview

Hygraph provides a native GraphQL Content API that serves as the primary interface for querying and manipulating content in your Hygraph project. Every Hygraph project exposes a unique GraphQL endpoint that reflects your content schema automatically.

## Endpoint Format

Each project receives a unique API endpoint based on its region and project identifier:

```
https://api-{region}.hygraph.com/v2/{projectId}/master
```

A read-only, high-performance CDN cache endpoint is also available:

```
https://api-{region}.hygraph.com/v2/{projectId}/master?gcms-stage=PUBLISHED
```

CDN-cached responses are not rate-limited and offer 3–5x lower latency compared to uncached requests.

## API Types

- **Content API** — Query and mutate content entries, assets, and relations.
- **Management API** — Programmatically manage project schema, models, fields, and environments.
- **Asset Upload API** — Upload binary assets (images, documents, videos) to Hygraph-managed storage.
- **MCP Server API** — Secure communication channel for AI assistants and automation tooling.

## Operations

### Queries

- Fetch single or paginated lists of content entries by model.
- Filter, sort, and paginate using auto-generated `where`, `orderBy`, `first`, `skip`, and `after` arguments.
- Query rich text, assets, references, remote sources, and federated content in a single request.

### Mutations

- `create{Model}` — Create a new content entry.
- `update{Model}` — Update an existing entry by `id`.
- `delete{Model}` — Delete an entry by `id`.
- `publish{Model}` / `unpublish{Model}` — Move entries through the publishing workflow.
- `upsert{Model}` — Create or update in a single operation.

### Subscriptions

Real-time GraphQL subscriptions are available on Growth and Enterprise plans, enabling live content updates pushed to clients.

## Authentication

API requests are authenticated via **Permanent Auth Tokens** or **Public API Tokens** passed as an `Authorization: Bearer <token>` header. Token scopes can be restricted to specific models or operations.

## API Playground

Every Hygraph project includes a built-in API Playground (GraphiQL) accessible from the project dashboard. The playground supports introspection, query auto-completion, and variable input for testing queries and mutations against live project data.

## Content Environments

Multiple content environments (staging, production, etc.) allow teams to develop schema changes and content in isolation. Each environment exposes its own API endpoint variant.

## Remote Sources & Content Federation

Hygraph's Content Federation feature allows embedding remote GraphQL or REST API data as virtual fields in the Hygraph schema. Remote sources are stitched into the unified GraphQL API without data duplication.

## References

- API Reference: https://hygraph.com/docs/api-reference
- GraphQL Content API Docs: https://hygraph.com/docs/api-reference/content-api
- Management API Docs: https://hygraph.com/docs/api-reference/management-api
- Rate Limits & API Limits: https://hygraph.com/docs/api-reference/basics/api-limits
