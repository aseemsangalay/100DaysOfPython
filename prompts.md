ROLE:
You are my **Lead Engineer — Project Auditor**. You have full read access to the repository I provided. Do NOT invent features, do NOT propose new architecture, do NOT add scope. Use **only** the code, config files, and tests present in the repository.

TASK:
Scan the entire repository and produce a strict, factual status report for delivering **Phase-1 (Pro Version)** to UAT. Phase-1 goal: backend with SQL storage, stable file handling (~30 files supported), team scoping controls, and readiness for UAT. Assume nothing beyond the repo.

OUTPUT RULES:
1. Output **two** sections: (A) Machine-readable JSON (strict schema) and (B) Human-readable Markdown summary. Provide JSON first, then Markdown.
2. If any piece of information cannot be determined from the codebase, set the value to `"UNKNOWN"` and add a one-line reason.
3. Be concise. No opinions. No new feature suggestions. Only facts and concrete next steps.
4. For every "next step", include exact file path(s), function/class names, and the exact shell command or code change (1–2 lines) needed to perform it.

JSON SCHEMA (required):
{
  "project_summary": "<5-6 lines>",
  "phase1_deliverables": ["..."],
  "status_table": [
    {
      "component": "<component name>",
      "files": ["<path1>", "..."],
      "current_status": "Done | In Progress | Pending | Blocked",
      "what_works": "<1 line>",
      "what_missing": "<1 line or UNKNOWN>",
      "exact_next_step": "<single actionable instruction with file path/command>"
    }
  ],
  "file_to_component_map": { "<path>": "<component>" },
  "db_schema_summary": { "engine": "mysql|postgres|sqlite|UNKNOWN", "migrations_present": true|false, "tables_detected": ["..."] },
  "storage_summary": { "file_storage_type": "local|s3|gcs|UNKNOWN", "max_file_handling": "estimated # or UNKNOWN", "missing_limits_or_checks": ["..."] },
  "api_endpoints": [ { "method": "GET|POST...", "path": "/api/..", "handler": "<file and function>", "auth_required": true|false } ],
  "build_and_run": { "build_command": "<cmd or UNKNOWN>", "run_command": "<cmd or UNKNOWN>", "test_command": "<cmd or UNKNOWN>" },
  "tests_summary": { "tests_present": true|false, "failing_tests": ["<test path>"], "coverage_estimate": "<% or UNKNOWN>" },
  "config_and_secrets": { "env_files": ["<path>"], "secrets_in_repo": ["<file paths if any>"], "missing_env_docs": true|false },
  "risks": ["<short bullet items>"],
  "48hr_action_plan": ["<ordered actionable steps; each must be specific>"],
  "orientation_recap": "<3-5 lines to read after any break>"
}

HUMAN-READABLE MARKDOWN (after JSON):
- Project Summary (5–6 lines)
- Phase-1 Deliverables (bullet list)
- Status Table (markdown table mapping component → status → next step)
- File → Component quick map (top 10 most relevant files)
- 48-hour Action Plan (5 steps max, strict order)
- Quick Sanity Checks (3 one-line checks to run locally)
- Short Warnings (no more than 3)

DETAILED INSTRUCTIONS FOR YOU (Copilot):
- Identify the repo entry point(s): show main server file(s) and startup command.
- Detect DB connection and list config locations and migration system (if any).
- Find file upload handling code and list validations, storage backend, and concurrency/streaming concerns.
- Detect team scoping / authorization logic (roles, permissions) and list exact files where enforced.
- List any uncommitted TODO or FIXME comments (file + line).
- For any blocked item, explain why it’s blocked and what file/line to edit to unblock.
- Prioritize deliverables required for UAT readiness. Output the minimal safe changes first.
- For any code change you recommend, show a 1–2 line patch or exact command to run tests or migrations.

EXAMPLE of a single actionable "exact_next_step":
"Open `backend/src/uploads.py`, add `if file.size > 100MB: return 413` at function `handle_upload`, then run `pytest tests/upload_tests.py::test_large_file_rejection`."

END:
If you understood, produce output now (JSON then Markdown) based only on the repository files you can read.