---
name: Run a RunWhen diagnostic runbook
description: Kick off a runbook run against an SLX, start it, and collect the report and any issues it surfaces.
api: openapi/runwhen-papi-openapi-original.json
operations:
  - create_run_request_api_v1_workspaces__workspace_name__slxs__slx_short_name__runbook_runs_post
  - start_run_request_api_v1_workspaces__workspace_name__slxs__slx_short_name__runbook_runs__run_request_id__start_post
  - get_run_request_api_v1_workspaces__workspace_name__slxs__slx_short_name__runbook_runs__run_request_id__get
  - get_run_request_report_api_v1_workspaces__workspace_name__slxs__slx_short_name__runbook_runs__run_request_id__report_get
  - get_run_request_issues_api_v1_workspaces__workspace_name__slxs__slx_short_name__runbook_runs__run_request_id__issues_get
---

# Run a RunWhen diagnostic runbook

Base URL: `https://papi.beta.runwhen.com`. Send `Authorization: Bearer <JWT>` on every call.
A runbook is attached to an SLX (Service Level Indicator eXperience), so you address it as
`.../workspaces/{workspace_name}/slxs/{slx_short_name}/runbook_runs`.

## Steps
1. **Create a run request** —
   `POST /api/v1/workspaces/{workspace_name}/slxs/{slx_short_name}/runbook_runs`
   (`create_run_request...`). Capture the returned `run_request_id`.
2. **Start it** — `POST .../runbook_runs/{run_request_id}/start` (`start_run_request...`).
3. **Poll status** — `GET .../runbook_runs/{run_request_id}` (`get_run_request...`) until the run
   completes.
4. **Read the report** — `GET .../runbook_runs/{run_request_id}/report`
   (`get_run_request_report...`).
5. **Collect surfaced issues** — `GET .../runbook_runs/{run_request_id}/issues`
   (`get_run_request_issues...`) then hand off to the triage skill.

## Rules
- On `502`, the agent runner / AgentFarm is unavailable — retry the start with backoff and check
  `https://runwhen.statuspage.io`.
- Poll rather than assuming synchronous completion; runs are asynchronous.
