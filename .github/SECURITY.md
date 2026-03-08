# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in any skill, please report it responsibly:

1. **Do not** open a public issue
2. Email the maintainer or use GitHub's private vulnerability reporting (Security tab > Report a vulnerability)
3. Include steps to reproduce and potential impact

## Scope

Skills in this repo are prompt-based instructions for Claude Code. They do not execute arbitrary code on their own, but they may include `scripts/` that run via shell. All scripts are reviewed before merge.

## What to Watch For

- Skills that instruct Claude to disable safety checks or bypass permissions
- Scripts with hardcoded credentials or tokens
- Skills that exfiltrate data or phone home to external services
- Prompt injection patterns embedded in skill instructions
