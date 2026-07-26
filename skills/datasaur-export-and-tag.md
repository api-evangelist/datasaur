---
name: Export Datasaur projects and manage tags
description: List projects, export their labels, and add or replace project tags via the Datasaur GraphQL API.
api: graphql (https://app.datasaur.ai/graphql)
operations: [getProjects, exportProject, updateProjectTags, removeProjectTags]
---

# Export Datasaur projects and manage tags

Use this skill to pull labeled data out of Datasaur and keep project tags tidy.

## Prerequisites
- OAuth2 `client_id` / `client_secret` (see https://docs.datasaur.ai/api/credentials)
- `api_url` (e.g. `https://app.datasaur.ai`)

## Steps
1. **Authenticate.** Exchange client credentials at `https://datasaur.ai/api/oauth/token` for a bearer `access_token`.
2. **List projects** with `getProjects` (paginated) — filter by `teamId` and `tags` to select a set.
3. **Export each project** with `exportProject`, passing the `project_id`, an output `filename`, and an `export_format` from the supported list.
4. **Manage tags** with `updateProjectTags`:
   - **PUT** replaces *all* tags with the supplied set.
   - **PATCH** adds new tags, keeping existing ones.
5. **(Optional) Clean up** with `removeProjectTags` — e.g. remove a batch tag after a successful export.

## Rules
- `getProjects` returns a paginated response; page through it rather than assuming one page.
- `project_id` is the resource ID in the project URL (e.g. `YOfkM6jKHzN`).
- Errors surface in the GraphQL `errors[]` array.
- For recurring bulk export, the Robosaur CLI (`export-projects`) wraps these operations with retry/state tracking.

## References
- conventions/datasaur-conventions.yml
- cli/datasaur-cli.yml
