# Hygraph GraphQL API

Hygraph (formerly GraphCMS) is a GraphQL-native headless CMS that auto-generates a full GraphQL Content API from your project's content schema. Every content model you define becomes a set of queries, mutations, and real-time subscriptions with built-in filtering, ordering, pagination, localization, and content staging.

## Authentication

Pass a Permanent Auth Token (PAT) or Public API key as a Bearer token in the `Authorization` header:

```
Authorization: Bearer <token>
```

Public content endpoints can be configured to allow unauthenticated access for published content via the CDN cache endpoint.

## Endpoint

**Standard endpoint (read/write, all stages):**
```
https://api-{region}.hygraph.com/v2/{projectId}/master
```

**CDN cache endpoint (high-performance, published content only):**
```
https://api-{region}.hygraph.com/v2/{projectId}/master?gcms-stage=PUBLISHED
```

Where `{region}` is the AWS region (e.g., `us-east-1`, `eu-central-1`) and `{projectId}` is your Hygraph project identifier. CDN-cached responses offer significantly lower latency and are not rate-limited.

**Management API:**
```
https://management.hygraph.com/graphql
```

## API Design

The Content API is project-specific. For each content model (e.g., `Post`), Hygraph auto-generates four query types and a set of mutations:

**Queries (per model):**
- `post(where: PostWhereUniqueInput!, stage: Stage!, locales: [Locale!]!): Post` — fetch single entry
- `posts(where: PostWhereInput, orderBy: PostOrderByInput, skip: Int, first: Int, last: Int, after: String, before: String, locales: [Locale!]!, stage: Stage!): [Post!]!` — fetch multiple entries
- `postVersion(where: VersionWhereInput!): DocumentVersion` — fetch specific revision
- `postsConnection(...): PostConnection!` — Relay cursor-based pagination with edges and pageInfo

**Mutations (per model):**
- `createPost(data: PostCreateInput!): Post!`
- `updatePost(where: PostWhereUniqueInput!, data: PostUpdateInput!): Post`
- `deletePost(where: PostWhereUniqueInput!): Post`
- `upsertPost(where: PostWhereUniqueInput!, upsert: PostUpsertInput!): Post!`
- `publishPost(where: PostWhereUniqueInput!, to: [Stage!]!, locales: [Locale!]): Post`
- `unpublishPost(where: PostWhereUniqueInput!, from: [Stage!]!, locales: [Locale!]): Post`
- `updateManyPostsConnection(where: PostManyWhereInput, data: PostUpdateManyInput!): PostConnection!`
- `deleteManyPostsConnection(where: PostManyWhereInput): PostConnection!`
- `publishManyPostsConnection(where: PostManyWhereInput, to: [Stage!]!, locales: [Locale!]): PostConnection!`
- `unpublishManyPostsConnection(where: PostManyWhereInput, from: [Stage!]!, locales: [Locale!]): PostConnection!`

## System (Built-in) Types

Every Hygraph project includes the following built-in types regardless of content model configuration:

- `Asset` — file storage type (images, video, documents, audio); localized by default; cannot be deleted from schema
- `User` — represents members and API token identities who create/update/publish content
- `ScheduledOperation` — a pending content change associated with a scheduled release
- `ScheduledRelease` — a named batch of scheduled operations to publish/unpublish at a future time
- `DocumentVersion` — a point-in-time snapshot of a content entry (revision history)
- `Version` — identifies a specific document version by id, stage, and revision number
- `PageInfo` — Relay cursor pagination metadata
- `BatchPayload` — returned by bulk mutation operations; contains `count` of affected records
- `RichText` — rich text field returning content as raw AST, html, markdown, text, and json

## Content Stages

Every project has `DRAFT` (default) and `PUBLISHED` system stages. Paid plans support additional custom stages (e.g., `QA`, `REVIEW`). Stage is used as a query argument and stored on system fields.

## Localization

Locale values (e.g., `en`, `de`, `es`) are passed as the `locales` argument on queries. The `Locale` enum is auto-generated from your project's configured locales. The `gcms-locales` HTTP header can also set locale fallback chains.

**References:**
- Documentation: https://hygraph.com/docs/api-reference/content-api
- GettingStarted: https://hygraph.com/docs
- Management API: https://hygraph.com/docs/api-reference/management-api
- Filtering: https://hygraph.com/docs/api-reference/content-api/filtering
- Ordering: https://hygraph.com/docs/api-reference/content-api/ordering
- Pagination: https://hygraph.com/docs/api-reference/content-api/pagination
- ContentStages: https://hygraph.com/docs/api-reference/content-api/content-stages
- Localization: https://hygraph.com/docs/api-reference/content-api/localization
- Assets: https://hygraph.com/docs/api-reference/content-api/assets
- GitHubOrganization: https://github.com/hygraph
