---
tracker:
  kind: linear
  project_slug: "test-project"
  active_states:
    - Todo
    - In Progress
  terminal_states:
    - Closed
    - Cancelled
    - Canceled
    - Duplicate
    - Done
polling:
  interval_ms: 5000
workspace:
  root: ~/code/symphony-test-workspaces
hooks:
  after_create: |
    git clone --depth 1 https://github.com/Zollicoff/discord-status-fusion.git .
agent:
  max_concurrent_agents: 1
  max_turns: 1
codex:
  command: codex --config shell_environment_policy.inherit=all --config 'model="gpt-5.5"' --config model_reasoning_effort=xhigh app-server
  approval_policy: never
  thread_sandbox: workspace-write
  turn_sandbox_policy:
    type: workspaceWrite
---

You are running a Symphony smoke test for Linear ticket `{{ issue.identifier }}`.

Issue context:
Identifier: {{ issue.identifier }}
Title: {{ issue.title }}
Current status: {{ issue.state }}
Labels: {{ issue.labels }}
URL: {{ issue.url }}

Description:
{% if issue.description %}
{{ issue.description }}
{% else %}
No description provided.
{% endif %}

Instructions:

1. This is a first-run smoke test, not a production workflow.
2. Work only in the provided repository copy.
3. Do not push branches, open pull requests, merge changes, or touch paths outside the workspace.
4. Use Symphony's `linear_graphql` tool to keep the Linear ticket updated.
5. If the ticket is `Todo`, move it to `In Progress` before starting work.
6. Create or update one Linear comment headed `## Symphony Smoke Test Workpad`.
7. Record the workspace path, current git SHA, what you inspected or changed, and validation results in that workpad.
8. If the ticket asks for a code or docs change, make the smallest safe local change and run the most relevant local validation available.
9. If the ticket is only asking for a smoke test, inspect the repository, run a harmless validation command if available, update the workpad, and move the ticket to `Done`.
10. If blocked by missing tools, auth, or unclear requirements, update the workpad with the blocker and leave the ticket in `In Progress`.

Final response must be concise and include only completed actions and blockers.
