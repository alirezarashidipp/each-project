# AGENTS.md

## Project

This repository contains a production application.

Before making changes, understand the existing architecture
and follow existing patterns.

## Repository Map

Architecture:
- docs/ARCHITECTURE.md

Coding conventions:
- docs/conventions.md

Testing:
- docs/testing.md

Security:
- docs/security.md

Deployment:
- docs/deployment.md

## Skills

Use the relevant skill before performing specialized work:

- Backend → skills/backend/SKILL.md
- Frontend → skills/frontend/SKILL.md
- Database → skills/database/SKILL.md
- Testing → skills/testing/SKILL.md
- Debugging → skills/debugging/SKILL.md
- Security → skills/security/SKILL.md
- Deployment → skills/deployment/SKILL.md

## General Rules

- Read relevant code before modifying it.
- Follow existing architecture.
- Prefer minimal changes.
- Do not duplicate existing functionality.
- Do not introduce unnecessary dependencies.
- Never hardcode secrets.
- Preserve backward compatibility unless explicitly requested.
- Add or update tests for behavioral changes.
- Update documentation when architecture or behavior changes.

## Before Coding

1. Understand the request.
2. Inspect related files.
3. Check existing implementation.
4. Read relevant documentation.
5. Read the relevant skill.
6. Identify tests that cover the area.

## After Coding

Run:

./scripts/check.sh

The task is not complete until required checks pass.

## Verification

`check.sh` should include:

- formatting
- linting
- type checking
- unit tests
- integration tests where relevant

If a check fails because of your change, fix it.

## Git

Before finishing:

- inspect git diff
- ensure unrelated files were not changed
- ensure secrets were not added
- ensure generated files are intentional


## Git Hooks

Local Git hooks are used to enforce checks automatically.

Before every push, the pre-push hook must run:

./scripts/check.sh

If the checks fail, the push must be blocked.

Do not bypass Git hooks unless explicitly instructed.



## Do Not

- Do not rewrite unrelated code.
- Do not silently change public APIs.
- Do not disable tests to make them pass.
- Do not suppress lint/type errors without justification.
- Do not commit secrets.
