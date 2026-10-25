# Personal setup and usage

This package contains review instructions, not a Python package. It requires no pip/npm installation, MCP connection, additional agents, or new credentials. Use it only where company policy permits personal custom skills and these instructions.

## Files

- SKILL.md: how Claude should review, draw the flow, handle uncertainty, and format findings.
- references/pipeline-checks.md: conditional checks for the pipeline technologies.
- USAGE.md: this human setup guide; you do not need to copy it into the installed skill.

## Install outside all work repositories

On the machine where Claude Code runs, extract pipeline-review.zip into a Downloads or temporary directory outside your work repos. Open a terminal inside the extracted pipeline-review folder containing SKILL.md, then run:

```bash
# Verify the extracted files are here.
pwd
ls SKILL.md references/pipeline-checks.md

# Create your personal location, outside work repos.
echo "$HOME"
mkdir -p "$HOME/.claude/skills/pipeline-review/references"

# Copy the two files. -i asks before replacing an existing file.
cp -i SKILL.md "$HOME/.claude/skills/pipeline-review/SKILL.md"
cp -i references/pipeline-checks.md "$HOME/.claude/skills/pipeline-review/references/pipeline-checks.md"

# Verify both files exist.
ls -l "$HOME/.claude/skills/pipeline-review/SKILL.md" "$HOME/.claude/skills/pipeline-review/references/pipeline-checks.md"
```

This uses your actual home directory; do not assume /home/code is your home. The resulting structure is:

```text
$HOME/.claude/skills/pipeline-review/
├── SKILL.md
└── references/
    └── pipeline-checks.md
```

Do not put the skill inside a work repo. No Git commit, push, or master/main merge is needed. Open Claude Code in a work repo and invoke /pipeline-review. Start a new session if necessary. Organization-managed settings may restrict personal skills; personal placement does not override company policy.

## First review

```text
/pipeline-review Review the current branch against [actual base branch].
Ticket/acceptance criteria: [provide text or approved link].
Route: [source → Iceberg OR source → S3 → Fivetran → Snowflake].
Include the overall flow diagram, evidence-backed findings, and unknowns.
Review only. Do not edit, commit, post, approve, or merge anything.
```

Provide available schemas, engine/connector versions, approved runbooks, and sanitized failure examples. Unknown requirements must remain unknown. The skill contains Python-specific checks but must discover your actual frameworks and test commands.

## Follow-up comments

```text
Convert findings F1 and F2 into review comments: file name, exact lines,
concise comment, suggested fix, and verification status.
Recheck the current head and locations. Do not post or apply anything.
```

In a new chat provide the previous report and PR/diff, or request a fresh review. The skill does not provide memory across chats.

## If installation is unavailable

If approved prompts are allowed, paste the body of SKILL.md and the relevant pipeline-checks.md sections into the review conversation. This supplies instructions but no unavailable tools or credentials. Manually copying external material still requires compliance with company policy.

## Evaluation and customization

Frontmatter, reference paths, and ZIP integrity were checked. This recreation has not been behaviorally evaluated on real PRs; no skill guarantees all edge cases or zero inference. Keep human judgment of findings and merge decisions.

Before relying on it, try a historical buggy PR without disclosing the known bug, a clean PR, and a PR missing important connector configuration. Check both missed bugs and false positives. The reviewer should recognize actual guards and label missing information without fabricating defects.

Add your real source-ordering/duplicate contracts, naming/type rules, checkpoint ownership, approved test commands, and runbook references. Do not invent team defaults.

## Sources previously consulted

- https://github.com/cursor/plugins/tree/main/pstack
- https://github.com/cursor/plugins/tree/main/pstack/skills/interrogate
- https://github.com/cursor/plugins/tree/main/pstack/skills/blast-radius
- https://github.com/cursor/plugins/tree/main/pstack/skills/principle-foundational-thinking
- https://github.com/cursor/plugins/tree/main/pstack/skills/principle-prove-it-works
- https://code.claude.com/docs/en/skills

These instructions are standalone and do not download or run pstack.
