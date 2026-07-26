---
name: Create a Datasaur labeling project
description: Authenticate, then create a new labeling project (with a guideline) using the Datasaur GraphQL API.
api: graphql (https://app.datasaur.ai/graphql)
operations: [createProject, createGuideline]
---

# Create a Datasaur labeling project

Use this skill to stand up a new labeling project on Datasaur.

## Prerequisites
- OAuth2 `client_id` and `client_secret` generated at https://docs.datasaur.ai/api/credentials
- Your `api_url` (SaaS: `https://app.datasaur.ai`; adjust for self-hosted)

## Steps
1. **Get an access token.** POST `client_id`, `client_secret`, and `grant_type=client_credentials` to `https://datasaur.ai/api/oauth/token`. Read `access_token` (expires in 3600s).
2. **Send the request** to `<api_url>/graphql` with headers `Authorization: Bearer <access token>` and `Content-Type: application/json` (use a multipart POST when uploading project files).
3. **Create the project** with the `createProject` mutation. This is asynchronous — the response describes a *job*, not the finished project.
4. **(Optional) Attach a guideline** with `createGuideline`.
5. **Poll job status** separately until the project is ready before exporting or labeling.

## Rules
- The legacy `launchTextProjectAsync` mutation is deprecated (support ended 2024-06-30); always use `createProject`.
- Errors arrive in the GraphQL top-level `errors[]` array — inspect it before assuming success.
- No idempotency key is available; because creation is async + job-tracked, avoid blind retries. For bulk creation prefer the Robosaur CLI, which keeps a state file.

## References
- conventions/datasaur-conventions.yml
- authentication/datasaur-authentication.yml
