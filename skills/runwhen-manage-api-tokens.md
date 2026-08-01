---
name: Manage RunWhen API tokens
description: Create, list, and revoke personal/service API tokens for automating the RunWhen Platform API.
api: openapi/runwhen-papi-openapi-original.json
operations:
  - create_token_api_v3_users_tokens_create_post
  - list_tokens_api_v3_users_tokens_get
  - revoke_token_api_v3_users_tokens__token_id__revoke_post
  - delete_token_api_v3_users_tokens__token_id__delete
---

# Manage RunWhen API tokens

Base URL: `https://papi.beta.runwhen.com`. Bootstrap auth with a JWT from
`POST /api/v3/token/` (or an existing token), sent as `Authorization: Bearer <JWT>`.

## Steps
1. **Create a token** — `POST /api/v3/users/tokens/create` (`create_token...`). Store the returned
   secret securely; it is shown once.
2. **List tokens** — `GET /api/v3/users/tokens` (`list_tokens...`) to audit active tokens.
3. **Revoke** — `POST /api/v3/users/tokens/{token_id}/revoke` (`revoke_token...`) to disable a
   token, or `DELETE /api/v3/users/tokens/{token_id}` (`delete_token...`) to remove it.

## Rules
- For unattended automation prefer **workspace service-account tokens**
  (`/api/v3/workspaces/{workspace}/service-account-tokens`) with scoped permissions and explicit
  expiry over personal tokens.
- Auth model: JWT Bearer only (no per-endpoint OAuth scopes on resource endpoints). The OAuth/OIDC
  server (scopes openid/profile/email) is for interactive login. See
  `authentication/runwhen-authentication.yml` and `scopes/runwhen-scopes.yml`.
