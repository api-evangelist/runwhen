---
name: Triage a RunWhen workspace issue
description: Investigate, summarize, and resolve an Issue detected in a RunWhen workspace using the Platform API.
api: openapi/runwhen-papi-openapi-original.json
operations:
  - list_workspace_issues_api_v1_workspaces__workspace_name__issues_get
  - get_workspace_issue_api_v1_workspaces__workspace_name__issues__issue_id__get
  - generate_issue_summary_api_v1_workspaces__workspace_name__issues__issue_id__generate_summary_post
  - generate_issue_next_steps_api_v1_workspaces__workspace_name__issues__issue_id__generate_next_steps_post
  - close_workspace_issue_api_v1_workspaces__workspace_name__issues__issue_id__close_patch
  - ignore_workspace_issue_api_v1_workspaces__workspace_name__issues__issue_id__ignore_patch
---

# Triage a RunWhen workspace issue

Base URL: `https://papi.beta.runwhen.com`. All calls require `Authorization: Bearer <JWT>`
(obtain via `POST /api/v3/token/`, or use a workspace service-account token). Resources are
workspace-scoped, so `{workspace_name}` is in every path.

## Steps
1. **List open issues** — `GET /api/v1/workspaces/{workspace_name}/issues`
   (`list_workspace_issues...`). Filter to the issue you want to triage.
2. **Fetch the issue detail** — `GET /api/v1/workspaces/{workspace_name}/issues/{issue_id}`
   (`get_workspace_issue...`) to read severity, affected SLX, and context.
3. **Generate an AI summary** — `POST .../issues/{issue_id}/generate_summary`
   (`generate_issue_summary...`).
4. **Generate next steps** — `POST .../issues/{issue_id}/generate_next_steps`
   (`generate_issue_next_steps...`) to get recommended remediation actions.
5. **Resolve** — once handled, `PATCH .../issues/{issue_id}/close` (`close_workspace_issue...`),
   or `PATCH .../issues/{issue_id}/ignore` (`ignore_workspace_issue...`) if not actionable.

## Rules
- Errors are JSON `{"detail": ...}` (not RFC 9457). On `401` refresh the JWT; on `404` verify the
  workspace name and issue id; on `422` inspect `detail[]` validation errors. See
  `errors/runwhen-problem-types.yml`.
- No idempotency-key header is supported — do not blindly retry POSTs. See
  `conventions/runwhen-conventions.yml`.
