# Personal Python development skill

This is the implementation companion to pipeline-review. It can edit local source, configuration, tests, and relevant docs. It does not commit, push, open PRs, merge, deploy, or mutate external systems. Its instructions are not a technical permission sandbox. No additional package, plugin, MCP connection or credentials are required by this skill; your project may have its own approved dependencies.

## Personal installation

Where workplace policy permits personal custom skills, extract pipeline-develop.zip outside your work repositories. Open a terminal in the extracted pipeline-develop folder containing SKILL.md. Run on the machine where Claude Code runs:

```bash
ls SKILL.md references/implementation-checks.md
echo "$HOME"
mkdir -p "$HOME/.claude/skills/pipeline-develop/references"
cp -i SKILL.md "$HOME/.claude/skills/pipeline-develop/SKILL.md"
cp -i references/implementation-checks.md "$HOME/.claude/skills/pipeline-develop/references/implementation-checks.md"
ls -l "$HOME/.claude/skills/pipeline-develop/SKILL.md" "$HOME/.claude/skills/pipeline-develop/references/implementation-checks.md"
```

-i asks before replacing existing files. Keep the skill in your home directory, not inside /home/code/<work-repo>. Nothing in the skill needs committing or merging. The Python code changes you ask Claude to make will still appear in the work repo as local changes for you to review.

Open Claude Code inside the relevant work repository. Invoke /pipeline-develop; start a fresh session if needed. Organization-managed settings may restrict personal skills. If custom skills are disallowed but approved prompts are permitted, use the instruction body and relevant reference sections as a prompt within policy. Personal placement does not override a restriction on external instructions.

## Build a new pipeline

```text
/pipeline-develop Build [source] → [Iceberg OR S3 → Fivetran → Snowflake].
Requirements: [ticket and acceptance criteria].
Use [existing pipeline path] as a convention reference.
Inspect the actual source and delivery contracts; label unknowns.
Show the flow, implement local code/tests, and report verification.
Do not commit, push, merge, deploy, or modify external systems.
```

## Add a column

```text
/pipeline-develop Add [column/type] to [pipeline].
Requirements: [requiredness/default and historical backfill requirement,
or say these are unknown]. Trace every affected layer.
Implement local changes and test old/new records. Show the updated flow
and any external schema/connector steps I must carry out separately.
```

## Fix a bug

```text
/pipeline-develop Fix [symptom] in [pipeline].
Evidence: [sanitized logs, sample IDs and affected time range].
Establish the root cause, reproduce where feasible, implement the fix,
and verify the regression. Explain any separate data recovery needed.
```

Provide actual schemas, source documentation, versions/settings, runbooks, and approved test commands where available. Do not share secrets. The skill should discover available evidence before asking questions and explicitly flag consequential unknowns rather than silently choosing source semantics.

## Review after development

If separately installed, ask pipeline-review to review the resulting local diff including uncommitted changes. Supply the actual base/revisions and ticket. Neither skill automatically posts, approves or merges. For a new chat provide the task/code and previous handoff; skills do not supply memory across chats.

## Package and evaluation

- SKILL.md: workflow, Python discipline, uncertainty, diagrams, verification and handoff.
- references/implementation-checks.md: route/task checks loaded only when relevant.
- USAGE.md: human instructions; only the first two files need installation.

Structural checks and archive integrity are checked during packaging. Behavioral effectiveness has not been evaluated on your repository. Try one bounded local change first, inspect the diff and tests, and check whether it identifies missing requirements without fabricating them. It cannot guarantee all edge cases or production readiness.

This is an original standalone skill informed by the pipeline requirements discussed and pstack themes; it does not install or execute pstack:
https://github.com/cursor/plugins/tree/main/pstack
